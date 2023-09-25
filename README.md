# Anthony Alvarez Delgado

![Proyect 1: Chess]
// 20230924180929
// https://raw.githubusercontent.com/TonyAlvaDB/Tony_Data_Analysis_Portfolio/main/Proyecto%201/Chess.ipynb

{
  "cells": [
    {
      "cell_type": "markdown",
      "id": "1de63f69-93cc-445c-8fff-21fe432221d3",
      "metadata": {
        
      },
      "source": [
        "# Chess games analysis"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "e0af5ef7-6b25-42e8-ad7d-b63a0059efd9",
      "metadata": {
        
      },
      "source": [
        "This is a set of just over 20,000 games collected from a selection of users on the site Lichess.org, and how to collect more. I will also upload more games in the future as I collect them."
      ]
    },
    {
      "cell_type": "markdown",
      "id": "93dc9792-669d-4a0f-8fea-7db44175fac1",
      "metadata": {
        
      },
      "source": [
        "Here is the description of the variables\n",
        "```\n",
        "id: Unique identifier for each chess game.\n",
        "rated: Indicates whether the game is rated or not (e.g., \"True\" for rated games).\n",
        "created_at: Timestamp for when the game was created.\n",
        "last_move_at: Timestamp for when the last move in the game was made.\n",
        "turns: The total number of moves made in the game.\n",
        "victory_status: Describes the status of the game's outcome (e.g., \"mate,\" \"resign,\" \"draw,\" etc.).\n",
        "winner: Indicates the winner of the game (e.g., \"white,\" \"black,\" or \"draw\").\n",
        "increment_code: The time control increment code for the game.\n",
        "white_id: Unique identifier for the white player in the game.\n",
        "white_rating: The rating of the white player.\n",
        "black_id: Unique identifier for the black player in the game.\n",
        "black_rating: The rating of the black player.\n",
        "moves: A list of moves made in the game, typically in algebraic notation.\n",
        "opening_eco: The opening's classification code (e.g., \"A00\" to \"E99\") based on the Encyclopedia of Chess Openings (ECO).\n",
        "opening_name: The name of the chess opening used in the game.\n",
        "opening_ply: The number of half-moves (plies) played in the opening phase of the game.\n",
        "rating_change: The difference between white piece player and black piece player\n",
        "```\n"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "f8246edb-df21-4fe6-81e5-84df4d4ba74a",
      "metadata": {
        
      },
      "source": [
        "For each of these separate games from Lichess. I collected this data using the Lichess API, which enables collection of any given users game history. The difficult part was collecting usernames to use, however the API also enables dumping of all users in a Lichess team. There are several teams on Lichess with over 1,500 players, so this proved an effective way to get users to collect games from."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 1,
      "id": "900f44ab-6a8f-4534-b16a-1f277d3dd339",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "import pandas as pd\n",
        "import numpy as np\n",
        "import matplotlib.pyplot as plt\n",
        "import seaborn as sns\n",
        "from scipy.stats import pearsonr"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 2,
      "id": "fa2e5a42-e0cf-4a23-9d04-0a60596cef00",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "df = pd.read_csv('games.csv')"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "cce0a57a-9032-4b0b-bb3c-4336fb17ea7d",
      "metadata": {
        
      },
      "source": [
        "## Deep look into data"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 4,
      "id": "3b2edb44-f814-4840-ab2c-3b0a2d52e113",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>id</th>\n",
              "      <th>rated</th>\n",
              "      <th>created_at</th>\n",
              "      <th>last_move_at</th>\n",
              "      <th>turns</th>\n",
              "      <th>victory_status</th>\n",
              "      <th>winner</th>\n",
              "      <th>increment_code</th>\n",
              "      <th>white_id</th>\n",
              "      <th>white_rating</th>\n",
              "      <th>black_id</th>\n",
              "      <th>black_rating</th>\n",
              "      <th>moves</th>\n",
              "      <th>opening_eco</th>\n",
              "      <th>opening_name</th>\n",
              "      <th>opening_ply</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>TZJHLljE</td>\n",
              "      <td>False</td>\n",
              "      <td>1.504210e+12</td>\n",
              "      <td>1.504210e+12</td>\n",
              "      <td>13</td>\n",
              "      <td>outoftime</td>\n",
              "      <td>white</td>\n",
              "      <td>15+2</td>\n",
              "      <td>bourgris</td>\n",
              "      <td>1500</td>\n",
              "      <td>a-00</td>\n",
              "      <td>1191</td>\n",
              "      <td>d4 d5 c4 c6 cxd5 e6 dxe6 fxe6 Nf3 Bb4+ Nc3 Ba5...</td>\n",
              "      <td>D10</td>\n",
              "      <td>Slav Defense: Exchange Variation</td>\n",
              "      <td>5</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>l1NXvwaE</td>\n",
              "      <td>True</td>\n",
              "      <td>1.504130e+12</td>\n",
              "      <td>1.504130e+12</td>\n",
              "      <td>16</td>\n",
              "      <td>resign</td>\n",
              "      <td>black</td>\n",
              "      <td>5+10</td>\n",
              "      <td>a-00</td>\n",
              "      <td>1322</td>\n",
              "      <td>skinnerua</td>\n",
              "      <td>1261</td>\n",
              "      <td>d4 Nc6 e4 e5 f4 f6 dxe5 fxe5 fxe5 Nxe5 Qd4 Nc6...</td>\n",
              "      <td>B00</td>\n",
              "      <td>Nimzowitsch Defense: Kennedy Variation</td>\n",
              "      <td>4</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>mIICvQHh</td>\n",
              "      <td>True</td>\n",
              "      <td>1.504130e+12</td>\n",
              "      <td>1.504130e+12</td>\n",
              "      <td>61</td>\n",
              "      <td>mate</td>\n",
              "      <td>white</td>\n",
              "      <td>5+10</td>\n",
              "      <td>ischia</td>\n",
              "      <td>1496</td>\n",
              "      <td>a-00</td>\n",
              "      <td>1500</td>\n",
              "      <td>e4 e5 d3 d6 Be3 c6 Be2 b5 Nd2 a5 a4 c5 axb5 Nc...</td>\n",
              "      <td>C20</td>\n",
              "      <td>King's Pawn Game: Leonardis Variation</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>kWKvrqYL</td>\n",
              "      <td>True</td>\n",
              "      <td>1.504110e+12</td>\n",
              "      <td>1.504110e+12</td>\n",
              "      <td>61</td>\n",
              "      <td>mate</td>\n",
              "      <td>white</td>\n",
              "      <td>20+0</td>\n",
              "      <td>daniamurashov</td>\n",
              "      <td>1439</td>\n",
              "      <td>adivanov2009</td>\n",
              "      <td>1454</td>\n",
              "      <td>d4 d5 Nf3 Bf5 Nc3 Nf6 Bf4 Ng4 e3 Nc6 Be2 Qd7 O...</td>\n",
              "      <td>D02</td>\n",
              "      <td>Queen's Pawn Game: Zukertort Variation</td>\n",
              "      <td>3</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>9tXo1AUZ</td>\n",
              "      <td>True</td>\n",
              "      <td>1.504030e+12</td>\n",
              "      <td>1.504030e+12</td>\n",
              "      <td>95</td>\n",
              "      <td>mate</td>\n",
              "      <td>white</td>\n",
              "      <td>30+3</td>\n",
              "      <td>nik221107</td>\n",
              "      <td>1523</td>\n",
              "      <td>adivanov2009</td>\n",
              "      <td>1469</td>\n",
              "      <td>e4 e5 Nf3 d6 d4 Nc6 d5 Nb4 a3 Na6 Nc3 Be7 b4 N...</td>\n",
              "      <td>C41</td>\n",
              "      <td>Philidor Defense</td>\n",
              "      <td>5</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "         id  rated    created_at  last_move_at  turns victory_status winner  \\\n",
              "0  TZJHLljE  False  1.504210e+12  1.504210e+12     13      outoftime  white   \n",
              "1  l1NXvwaE   True  1.504130e+12  1.504130e+12     16         resign  black   \n",
              "2  mIICvQHh   True  1.504130e+12  1.504130e+12     61           mate  white   \n",
              "3  kWKvrqYL   True  1.504110e+12  1.504110e+12     61           mate  white   \n",
              "4  9tXo1AUZ   True  1.504030e+12  1.504030e+12     95           mate  white   \n",
              "\n",
              "  increment_code       white_id  white_rating      black_id  black_rating  \\\n",
              "0           15+2       bourgris          1500          a-00          1191   \n",
              "1           5+10           a-00          1322     skinnerua          1261   \n",
              "2           5+10         ischia          1496          a-00          1500   \n",
              "3           20+0  daniamurashov          1439  adivanov2009          1454   \n",
              "4           30+3      nik221107          1523  adivanov2009          1469   \n",
              "\n",
              "                                               moves opening_eco  \\\n",
              "0  d4 d5 c4 c6 cxd5 e6 dxe6 fxe6 Nf3 Bb4+ Nc3 Ba5...         D10   \n",
              "1  d4 Nc6 e4 e5 f4 f6 dxe5 fxe5 fxe5 Nxe5 Qd4 Nc6...         B00   \n",
              "2  e4 e5 d3 d6 Be3 c6 Be2 b5 Nd2 a5 a4 c5 axb5 Nc...         C20   \n",
              "3  d4 d5 Nf3 Bf5 Nc3 Nf6 Bf4 Ng4 e3 Nc6 Be2 Qd7 O...         D02   \n",
              "4  e4 e5 Nf3 d6 d4 Nc6 d5 Nb4 a3 Na6 Nc3 Be7 b4 N...         C41   \n",
              "\n",
              "                             opening_name  opening_ply  \n",
              "0        Slav Defense: Exchange Variation            5  \n",
              "1  Nimzowitsch Defense: Kennedy Variation            4  \n",
              "2   King's Pawn Game: Leonardis Variation            3  \n",
              "3  Queen's Pawn Game: Zukertort Variation            3  \n",
              "4                        Philidor Defense            5  "
            ]
          },
          "execution_count": 4,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.head()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "16a56375-b507-4a77-a2f1-9de740b31cd3",
      "metadata": {
        
      },
      "source": [
        "## Check if there is any null value"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 6,
      "id": "7d4d4148-9731-49d7-a427-cadb0d255e19",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>id</th>\n",
              "      <th>rated</th>\n",
              "      <th>created_at</th>\n",
              "      <th>last_move_at</th>\n",
              "      <th>turns</th>\n",
              "      <th>victory_status</th>\n",
              "      <th>winner</th>\n",
              "      <th>increment_code</th>\n",
              "      <th>white_id</th>\n",
              "      <th>white_rating</th>\n",
              "      <th>black_id</th>\n",
              "      <th>black_rating</th>\n",
              "      <th>moves</th>\n",
              "      <th>opening_eco</th>\n",
              "      <th>opening_name</th>\n",
              "      <th>opening_ply</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "Empty DataFrame\n",
              "Columns: [id, rated, created_at, last_move_at, turns, victory_status, winner, increment_code, white_id, white_rating, black_id, black_rating, moves, opening_eco, opening_name, opening_ply]\n",
              "Index: []"
            ]
          },
          "execution_count": 6,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df[df.isnull().any(axis=1)]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 9,
      "id": "3a0d08db-59f2-40f2-903c-64c8be456291",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>id</th>\n",
              "      <th>rated</th>\n",
              "      <th>created_at</th>\n",
              "      <th>last_move_at</th>\n",
              "      <th>turns</th>\n",
              "      <th>victory_status</th>\n",
              "      <th>winner</th>\n",
              "      <th>increment_code</th>\n",
              "      <th>white_id</th>\n",
              "      <th>white_rating</th>\n",
              "      <th>black_id</th>\n",
              "      <th>black_rating</th>\n",
              "      <th>moves</th>\n",
              "      <th>opening_eco</th>\n",
              "      <th>opening_name</th>\n",
              "      <th>opening_ply</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>count</th>\n",
              "      <td>20058</td>\n",
              "      <td>20058</td>\n",
              "      <td>2.005800e+04</td>\n",
              "      <td>2.005800e+04</td>\n",
              "      <td>20058.000000</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058.000000</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058.000000</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058</td>\n",
              "      <td>20058.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>unique</th>\n",
              "      <td>19113</td>\n",
              "      <td>2</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>4</td>\n",
              "      <td>3</td>\n",
              "      <td>400</td>\n",
              "      <td>9438</td>\n",
              "      <td>NaN</td>\n",
              "      <td>9331</td>\n",
              "      <td>NaN</td>\n",
              "      <td>18920</td>\n",
              "      <td>365</td>\n",
              "      <td>1477</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>top</th>\n",
              "      <td>XRuQPSzH</td>\n",
              "      <td>True</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>resign</td>\n",
              "      <td>white</td>\n",
              "      <td>10+0</td>\n",
              "      <td>taranga</td>\n",
              "      <td>NaN</td>\n",
              "      <td>taranga</td>\n",
              "      <td>NaN</td>\n",
              "      <td>e4 e5</td>\n",
              "      <td>A00</td>\n",
              "      <td>Van't Kruijs Opening</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>freq</th>\n",
              "      <td>5</td>\n",
              "      <td>16155</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>11147</td>\n",
              "      <td>10001</td>\n",
              "      <td>7721</td>\n",
              "      <td>72</td>\n",
              "      <td>NaN</td>\n",
              "      <td>82</td>\n",
              "      <td>NaN</td>\n",
              "      <td>27</td>\n",
              "      <td>1007</td>\n",
              "      <td>368</td>\n",
              "      <td>NaN</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>mean</th>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1.483617e+12</td>\n",
              "      <td>1.483618e+12</td>\n",
              "      <td>60.465999</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1596.631868</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1588.831987</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>4.816981</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>std</th>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>2.850151e+10</td>\n",
              "      <td>2.850140e+10</td>\n",
              "      <td>33.570585</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>291.253376</td>\n",
              "      <td>NaN</td>\n",
              "      <td>291.036126</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>2.797152</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>min</th>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1.376772e+12</td>\n",
              "      <td>1.376772e+12</td>\n",
              "      <td>1.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>784.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>789.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>25%</th>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1.477548e+12</td>\n",
              "      <td>1.477548e+12</td>\n",
              "      <td>37.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1398.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1391.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>3.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>50%</th>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1.496010e+12</td>\n",
              "      <td>1.496010e+12</td>\n",
              "      <td>55.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1567.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1562.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>4.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>75%</th>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1.503170e+12</td>\n",
              "      <td>1.503170e+12</td>\n",
              "      <td>79.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1793.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1784.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>6.000000</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>max</th>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>1.504493e+12</td>\n",
              "      <td>1.504494e+12</td>\n",
              "      <td>349.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>2700.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>2723.000000</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>NaN</td>\n",
              "      <td>28.000000</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "              id  rated    created_at  last_move_at         turns  \\\n",
              "count      20058  20058  2.005800e+04  2.005800e+04  20058.000000   \n",
              "unique     19113      2           NaN           NaN           NaN   \n",
              "top     XRuQPSzH   True           NaN           NaN           NaN   \n",
              "freq           5  16155           NaN           NaN           NaN   \n",
              "mean         NaN    NaN  1.483617e+12  1.483618e+12     60.465999   \n",
              "std          NaN    NaN  2.850151e+10  2.850140e+10     33.570585   \n",
              "min          NaN    NaN  1.376772e+12  1.376772e+12      1.000000   \n",
              "25%          NaN    NaN  1.477548e+12  1.477548e+12     37.000000   \n",
              "50%          NaN    NaN  1.496010e+12  1.496010e+12     55.000000   \n",
              "75%          NaN    NaN  1.503170e+12  1.503170e+12     79.000000   \n",
              "max          NaN    NaN  1.504493e+12  1.504494e+12    349.000000   \n",
              "\n",
              "       victory_status winner increment_code white_id  white_rating black_id  \\\n",
              "count           20058  20058          20058    20058  20058.000000    20058   \n",
              "unique              4      3            400     9438           NaN     9331   \n",
              "top            resign  white           10+0  taranga           NaN  taranga   \n",
              "freq            11147  10001           7721       72           NaN       82   \n",
              "mean              NaN    NaN            NaN      NaN   1596.631868      NaN   \n",
              "std               NaN    NaN            NaN      NaN    291.253376      NaN   \n",
              "min               NaN    NaN            NaN      NaN    784.000000      NaN   \n",
              "25%               NaN    NaN            NaN      NaN   1398.000000      NaN   \n",
              "50%               NaN    NaN            NaN      NaN   1567.000000      NaN   \n",
              "75%               NaN    NaN            NaN      NaN   1793.000000      NaN   \n",
              "max               NaN    NaN            NaN      NaN   2700.000000      NaN   \n",
              "\n",
              "        black_rating  moves opening_eco          opening_name   opening_ply  \n",
              "count   20058.000000  20058       20058                 20058  20058.000000  \n",
              "unique           NaN  18920         365                  1477           NaN  \n",
              "top              NaN  e4 e5         A00  Van't Kruijs Opening           NaN  \n",
              "freq             NaN     27        1007                   368           NaN  \n",
              "mean     1588.831987    NaN         NaN                   NaN      4.816981  \n",
              "std       291.036126    NaN         NaN                   NaN      2.797152  \n",
              "min       789.000000    NaN         NaN                   NaN      1.000000  \n",
              "25%      1391.000000    NaN         NaN                   NaN      3.000000  \n",
              "50%      1562.000000    NaN         NaN                   NaN      4.000000  \n",
              "75%      1784.000000    NaN         NaN                   NaN      6.000000  \n",
              "max      2723.000000    NaN         NaN                   NaN     28.000000  "
            ]
          },
          "execution_count": 9,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.describe(include='all')"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 11,
      "id": "148d34e8-8992-4a66-9e58-2207546f8766",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "(20058, 16)"
            ]
          },
          "execution_count": 11,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.shape"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 12,
      "id": "c8f06bd8-d282-40f8-9b8c-d11d545b1c7a",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "name": "stdout",
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 20058 entries, 0 to 20057\n",
            "Data columns (total 16 columns):\n",
            " #   Column          Non-Null Count  Dtype  \n",
            "---  ------          --------------  -----  \n",
            " 0   id              20058 non-null  object \n",
            " 1   rated           20058 non-null  bool   \n",
            " 2   created_at      20058 non-null  float64\n",
            " 3   last_move_at    20058 non-null  float64\n",
            " 4   turns           20058 non-null  int64  \n",
            " 5   victory_status  20058 non-null  object \n",
            " 6   winner          20058 non-null  object \n",
            " 7   increment_code  20058 non-null  object \n",
            " 8   white_id        20058 non-null  object \n",
            " 9   white_rating    20058 non-null  int64  \n",
            " 10  black_id        20058 non-null  object \n",
            " 11  black_rating    20058 non-null  int64  \n",
            " 12  moves           20058 non-null  object \n",
            " 13  opening_eco     20058 non-null  object \n",
            " 14  opening_name    20058 non-null  object \n",
            " 15  opening_ply     20058 non-null  int64  \n",
            "dtypes: bool(1), float64(2), int64(4), object(9)\n",
            "memory usage: 2.3+ MB\n"
          ]
        }
      ],
      "source": [
        "df.info()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "59faca9a-516d-4809-babf-c1a79ef1cc47",
      "metadata": {
        
      },
      "source": [
        "With all of this we can see that all variables have 20058 non-null observations. Matches with the shape of the dataset (20058, 16). So we can say that dataset does not have any null value"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "b2643758-7110-4275-9739-68c03098b274",
      "metadata": {
        
      },
      "source": [
        "### What is the most winnable color of pieces?"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 19,
      "id": "f7ff7bcd-9b84-4624-8a92-c58272979ab3",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "white    0.498604\n",
              "black    0.454033\n",
              "draw     0.047363\n",
              "Name: winner, dtype: float64"
            ]
          },
          "execution_count": 19,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.winner.value_counts(normalize=True)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 61,
      "id": "78fcdd51-1b40-427f-885d-d432c925db3b",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "white    10001\n",
              "black     9107\n",
              "draw       950\n",
              "Name: winner, dtype: int64"
            ]
          },
          "execution_count": 61,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.winner.value_counts()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 29,
      "id": "6a49b696-e7a4-4425-8ec8-5172a3a22c66",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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9ZjbWq1WXnzxRTZt2sSsWbNYtGgRDz300OHH169fz/nnn89pp53G8uXLWbp0KZdffjlOp/Oo53r33Xe59tprmT17NjfddJPPXp+IdJ2mAxK/yM3NpbKyks2bN2OxWAB45JFH+Mc//sGWLVvIzMzknnvuOe6gg/fff59f/OIXVFVVAXDDDTdQUlLC0qVLj/vzsrKyGDZsGI899hjz5s1jypQpPnltItJ9EaYDSPg488wzDxcRwKRJk3juueeOuTdjt9v5/e9/z5YtW6irq8PhcNDS0kJjYyNxcXGsX7+eH/3oRyf8eXPnzqWiooKlS5dyxhlneP31iIj36DCdBJw9e/Zw6aWXMnr0aObOncuaNWv485//DEB7u2dJ8R49epz0ebKysujduzdvvPHGcc9NiUhgUBmJ36xYseKo26eeeio225HLLaxevRqHw8Fzzz3HmWeeybBhw9i7d+8R24wdO5aFCxee8OcNGTIEu93ORx99xJ133umdFyEiPqEyEr8pLS3lvvvuY/v27bzzzju89NJL3H333UdtN2TIEBwOBy+99BJFRUXMmTOHV1555YhtHn30UVatWsUvf/lLNmzYwLZt23j55ZcPn1P6xrBhw7Db7cydO1cXwYoEMJWR+M1NN91Ec3MzZ5xxBrfffjt33nknt95661HbZWVlMWPGDJ555hlGjx7N22+/zVNPPXXENsOGDWP+/PkUFBRwxhlnMGnSJD766CMiIo4+DTp8+HAWLVrEO++8w/333++z1yciXafRdCIiYpz2jERExDiVkYiIGKcyEhER41RGIiJinMpIRESMUxmJiIhxKiMRETFOZSQiIsapjERExDiVkYiIGKcyEhER41RGIiJinMpIRESMUxmJiIhxKiMRETFOZSQiIsapjERExDiVkYiIGKcyEhER41RGIiJinMpIRESMUxmJiIhxKiMRETFOZSQiIsapjERExDiVkYiIGKcyEhER41RGIiJi3P8H2LQiuoBsfCcAAAAASUVORK5CYII=",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "df.winner.value_counts().plot.pie()\n",
        "plt.show()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "4bd13895-b660-4fcc-a7c7-75437d64b4d5",
      "metadata": {
        
      },
      "source": [
        "There is not a huge difference, but white color pieces (49.86%) win more than black color pieces (45.40%) for %4.46. Draws correspond to 4.73%"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "a4995ee3-b4dd-4a45-a8e5-60c9eba020b0",
      "metadata": {
        
      },
      "source": [
        "## What is the most common way to win?"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 23,
      "id": "bf59dc47-88f7-43b9-a738-ed9c4e679e84",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "array(['outoftime', 'resign', 'mate', 'draw'], dtype=object)"
            ]
          },
          "execution_count": 23,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.victory_status.unique()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 31,
      "id": "f0a8c216-0b58-47e3-9c59-1407bf85615b",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "resign       0.555738\n",
              "mate         0.315336\n",
              "outoftime    0.083757\n",
              "draw         0.045169\n",
              "Name: victory_status, dtype: float64"
            ]
          },
          "execution_count": 31,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.victory_status.value_counts(normalize=True)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 30,
      "id": "abe15c75-6efb-4a18-b98d-22459967311e",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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evXqpXvuuUdBQUFV6r/44guVl5fr17/+tTXmdDoVGRlZpfbaa6+1/uxwOBQWFma9vrhwfvy8e3h4KDg42HprXfr+50yS9Vo899xz+te//qX9+/erpKREZWVl5/2gSFZWlvbu3auAgAC38e+++0779u2roZngfPbt26eysjLFxsZaY40aNary8/jjfw9Oe/XVVzVv3jzt3btXx48f16lTp9y+tDc+Pl7PP/+8KisrlZGRoe7du6t58+bKyMhQ586dtXv3bsXFxV24ydUBhKZLgPnhou6nz1H68bjD4VCDBg2smtNOv099vv2euc9zjf9cnp6ebvcdDsdZxyorKyVJlZWV1nvqHh4ebnVn++UPd35+ftafKysrdccdd2jmzJlV6po2bSoPDw+tWbNGGzZs0OrVq/Xss8/qkUce0ebNm9WqVSu3+nP9/TvTuV5fXDjn+1k7/dpVVlbqf//3fzV27FjNnj1bsbGxCggI0FNPPaXNmzef8xiVlZWKjo7W8uXLqzxW3f+44ec728/d2fz43wNJ2rRpk+69915NmzZNvXv3ltPp1IoVK9zOh7rllltUXFysrVu3au3atXriiScUERGhtLQ0derUSSEhIWrfvn2Nzqeu4UTwS0CbNm3k5eWldevWWWPl5eX65JNP1L59ezVp0kTFxcXWSbySlJ2d7bYPLy8vtxOupe9XlQ4cOKCDBw9aY5999plcLtc5fzDOtq+acP3116uiokIFBQVq06aN241P5v08nTt31s6dO9WyZcsqz+Xpf0wdDoe6deumadOmadu2bfLy8lJ6enqVfV199dXy9PRUZmamNVZUVKQ9e/ZctPmg5qxdu1Zdu3ZVSkqKrr/+erVp06bKStHZfsY7d+6sPXv2KCQkpMrfKT5tefG0adNGnp6e2rRpkzVWWFio3bt3n3O79evXq0WLFnrkkUd0ww03qG3bttq/f79bjdPpVKdOnTR//nw5HA516NBBN998s7Zt26a33nrrkl9lkghNlwQ/Pz89+OCDmjhxolatWqXPPvtMycnJOnnypIYPH66YmBj5+vrqT3/6k/bu3auXXnqpyjV2WrZsqdzcXGVnZ+vrr79WaWmpevTooWuvvVaDBw/W1q1blZmZqfvvv19xcXFnXdr98b42b96sL7/8Ul9//XWNrSS0a9dOgwcP1v3336/XX39dubm52rJli2bOnKmVK1fWyDEuFw899JC+/fZb/e53v1NmZqa++OILrV69WsOGDVNFRYU2b96stLQ0ffLJJzpw4IBef/11HT169KxhOSAgQEOGDNHEiRP14YcfaufOnRo2bJgaNGhQIyuSuLjatGmjTz75RO+++652796tRx991Pok7mktW7bUp59+ql27dunrr79WeXm5Bg8erMaNG6tfv35au3atcnNzlZGRoYcffliHDh2qpdlcfvz9/TV8+HBNnDhR77//vnbs2KGhQ4eqQYNz/7pv06aNDhw4oBUrVmjfvn3629/+dtb/JMXHx2vZsmWKi4uTw+FQUFCQOnTooFdeeUXx8fEXaFZ1B6HpEvHXv/5Vd999t5KSktS5c2ft3btX7777roKCgtSoUSMtW7ZMK1euVFRUlF5++WVNnTrVbfu7775bffr00a233qomTZro5ZdflsPh0BtvvKGgoCDdcsst6tGjh1q3bq1XXnnlnL1MmDBBHh4e6tChg5o0aaIDBw7U2DwXL16s+++/X+PHj1dkZKQSExO1efNmt/OucH7h4eFav369Kioq1Lt3b3Xs2FEPP/ywnE6nGjRooMDAQH388cfq27ev2rVrpz//+c+aPXu2brvttrPub86cOYqNjVVCQoJ69Oihbt26qX379rriiisu8szwS/3hD3/QXXfdpYEDByomJkbffPONUlJS3GqSk5MVGRmpG264QU2aNNH69evl6+urjz/+WM2bN9ddd92l9u3ba9iwYSopKXE7LwYX3lNPPaVbbrlFiYmJ6tGjh2666SZFR0efc5t+/fpp7NixGjVqlDp16qQNGzbo0UcfrVJ36623qqKiwi0gxcXFqaKi4rJYaXIYu2+AAoBNJ06c0FVXXaXZs2dr+PDhtd0OANQITgQH8Itt27ZNn3/+uX7961/L5XJp+vTpkr7/3ysAXCoITQBqxNNPP61du3bJy8tL0dHRWrt2rRo3blzbbQFAjeHtOQAAABs4ERwAAMAGQhMAAIANhCYAAAAbCE0AAAA2EJoAAABsIDQBqBOmTp2qTp061XYbAPCTCE0A6oQJEybo/ffft11/+mt+6rqWLVtq3rx5P3u7+Ph4paam1ng/AKqPi1sCqBP8/f3l7+9/0Y9bXl4uT0/Pi35cAPUPK00ALoqFCxfqqquuUmVlpdt4YmKihgwZcta351544QVdc8018vb2VtOmTTVq1ChJ36/eSNJvf/tbORwO674kLViwQFdffbW8vLwUGRmppUuXuu3T4XDoueeeU79+/eTn56e//OUvatOmjZ5++mm3uh07dqhBgwbat2/feec2depUNW/eXN7e3goPD9eYMWMkfb9atH//fo0dO1YOh0MOh0OS9M033+h3v/udmjVrJl9fX+uLtE8bOnSoMjIy9Mwzz1jbffnll1qyZImuvPJKt2O/8cYb1n4l6b///a9uvfVWBQQEKDAwUNHR0frkk0/OOwcA50doAnBR9O/fX19//bU+/PBDa6ywsFDvvvuuBg8eXKV+wYIFeuihhzRixAht375db775ptq0aSNJ2rJliyRp8eLFysvLs+6np6fr4Ycf1vjx47Vjxw6NHDlSv//9792OKUmPP/64+vXrp+3bt2vYsGEaNmyYFi9e7Fbzwgsv6Oabb9bVV199znm9+uqrmjt3rhYuXKg9e/bojTfeUFRUlCTp9ddfV7NmzTR9+nTl5eUpLy9PkvTdd98pOjpab731lnbs2KERI0YoKSlJmzdvliQ988wzio2NVXJysrVdRESEred58ODBatasmbZs2aKsrCz98Y9/ZCUNqCkGAC6SxMREM2zYMOv+woULTVhYmDl16pR5/PHHzXXXXWc9Fh4ebh555JGf3Jckk56e7jbWtWtXk5yc7DbWv39/07dvX7ftUlNT3WoOHz5sPDw8zObNm40xxpSVlZkmTZqYJUuWnHdOs2fPNu3atTNlZWVnfbxFixZm7ty5591P3759zfjx4637cXFx5uGHH3arWbx4sXE6nW5j6enp5sf/lAcEBNjqG8DPx0oTgItm8ODBeu2111RaWipJWr58ue699155eHi41RUUFOjw4cPq3r37z9p/Tk6OunXr5jbWrVs35eTkuI3dcMMNbvebNm2q22+/XS+88IIk6a233tJ3332n/v37n/eY/fv3V0lJiVq3bq3k5GSlp6fr1KlT59ymoqJCTz75pK699loFBwfL399fq1ev1oEDB+xM85zGjRunBx54QD169NBf//pXW28vArCH0ATgornjjjtUWVmpt99+WwcPHtTatWt13333Vanz8fGp9jF+fH6PJBljqoz5+flV2e6BBx7QihUrVFJSosWLF2vgwIHy9fU97/EiIiK0a9cu/f3vf5ePj49SUlJ0yy23qLy8/Ce3mT17tubOnatJkybpgw8+UHZ2tnr37q2ysrJzHqtBgwYyZ3zH+pnHmTp1qnbu3Knbb79dH3zwgTp06KD09PTzzgPA+RGaAFw0Pj4+uuuuu7R8+XK9/PLLateunaKjo6vUBQQEqGXLlue8BIGnp6cqKircxtq3b69169a5jW3YsEHt27c/b299+/aVn5+fFixYoHfeeUfDhg2zOavv55WYmKi//e1v+uijj7Rx40Zt375dkuTl5VWlz7Vr16pfv3667777dN1116l169bas2ePW83ZtmvSpImKi4t14sQJayw7O7tKP+3atdPYsWO1evVq3XXXXVXO1wJQPVxyAMBFNXjwYN1xxx3auXPnWVeZTps6dar+8Ic/KCQkRLfddpuKi4u1fv16jR49WpKsUNWtWzd5e3srKChIEydO1IABA9S5c2d1795d//nPf/T666/rvffeO29fHh4eGjp0qKZMmaI2bdooNjbW1nyWLFmiiooKxcTEyNfXV0uXLpWPj49atGhh9fnxxx/r3nvvlbe3txo3bqw2bdrotdde04YNGxQUFKQ5c+YoPz/fLdy1bNlSmzdv1pdffil/f381atTIOsaf/vQnjR49WpmZmVqyZIm1TUlJiSZOnKh77rlHrVq10qFDh7RlyxbdfffdtuYC4Dxq+6QqAJeXU6dOmaZNmxpJZt++fdb4mSeCG2PMc889ZyIjI42np6dp2rSpGT16tPXYm2++adq0aWMaNmxoWrRoYY3/4x//MK1btzaenp6mXbt25n/+53/c9qmznEB+2r59+4wkM2vWLNvzSU9PNzExMSYwMND4+fmZLl26mPfee896fOPGjebaa6813t7e1gnb33zzjenXr5/x9/c3ISEh5s9//rO5//77Tb9+/aztdu3aZbp06WJ8fHyMJJObm2sdr02bNuaKK64wCQkJZtGiRdZ+S0tLzb333msiIiKMl5eXCQ8PN6NGjTIlJSW25wPgpzmMOeMNcgC4TK1fv17x8fE6dOiQQkNDa7sdAHUMoQnAZa+0tFQHDx7UiBEj1LRpUy1fvry2WwJQB3EiOIDL3ssvv6zIyEi5XC7NmjXL7bHly5dbX/Fy5u2aa66ppY4B1AZWmgDgHIqLi3XkyJGzPubp6Wmd8A3g0kdoAgAAsIG35wAAAGwgNAEAANhAaAIAALCB0AQAAGADoQkAAMAGQhMAAIANhCYAAAAbCE0AAAA2/D8i0VbA2uX8IQAAAABJRU5ErkJggg==",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.countplot(x='victory_status', data=df)\n",
        "plt.show()\n",
        "plt.close()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "af6b20d9-1cb0-4038-bf54-8a6a707bb316",
      "metadata": {
        
      },
      "source": [
        "In this plot we can see that the most common way to win is **resign** (55.57%), followed by **mate** (31.53%). Then **outoftime** (8.37%), and lastly **draw** (4.51%). This, in numbers is:"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 33,
      "id": "539fcac4-0bb1-4edf-b130-290fdb118f26",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "resign       11147\n",
              "mate          6325\n",
              "outoftime     1680\n",
              "draw           906\n",
              "Name: victory_status, dtype: int64"
            ]
          },
          "execution_count": 33,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.victory_status.value_counts()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "63200fbb-c638-4404-a4a8-c0c33619a858",
      "metadata": {
        
      },
      "source": [
        "## What is the most common win oppening strategy for white and black pieces?"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 261,
      "id": "d78ea2a9-26f3-4919-9f71-bfdb4360d24d",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfwhite = df[df.winner == 'white']\n",
        "dfblack = df[df.winner == 'black']"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "9fefa337-0efe-4cf9-bf3f-d1484de43c42",
      "metadata": {
        
      },
      "source": [
        "### White"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 195,
      "id": "4591eb5e-d397-4675-a1d0-d76dd880e8df",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "10001"
            ]
          },
          "execution_count": 195,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "len(dfwhite.opening_name)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 158,
      "id": "6fb9f992-4d2a-40ad-99b7-3cba80cb2795",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "Scandinavian Defense: Mieses-Kotroc Variation    164\n",
              "Sicilian Defense                                 149\n",
              "Scotch Game                                      145\n",
              "French Defense: Knight Variation                 135\n",
              "Philidor Defense #3                              127\n",
              "Van't Kruijs Opening                             126\n",
              "Sicilian Defense: Bowdler Attack                 119\n",
              "Queen's Pawn Game: Mason Attack                  116\n",
              "Queen's Pawn Game: Chigorin Variation            112\n",
              "Horwitz Defense                                  110\n",
              "Name: opening_name, dtype: int64"
            ]
          },
          "execution_count": 158,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfwhite.opening_name.value_counts().head(10)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 166,
      "id": "4f34a294-f220-44b3-a0b1-b0f6c4137c23",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfgraphwhite = dfwhite[dfwhite.opening_name.isin(['Scandinavian Defense: Mieses-Kotroc Variation',\\\n",
        "                                                  'Sicilian Defense', \\\n",
        "                                                 'Scotch Game', \\\n",
        "                                                 'French Defense: Knight Variation', \\\n",
        "                                                 'Philidor Defense #3', \\\n",
        "                                                 \"Van't Kruijs Opening\", \\\n",
        "                                                 'Sicilian Defense: Bowdler Attack', \\\n",
        "                                                 \"Queen's Pawn Game: Mason Attack\", \\\n",
        "                                                 \"Queen's Pawn Game: Chigorin Variation\", \\\n",
        "                                                 'Horwitz Defense'])]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 172,
      "id": "7ccfa872-d077-4f63-9e3a-5ef398f84f0b",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "array(['Scandinavian Defense: Mieses-Kotroc Variation',\n",
              "       \"Van't Kruijs Opening\", 'Horwitz Defense',\n",
              "       'Sicilian Defense: Bowdler Attack',\n",
              "       \"Queen's Pawn Game: Chigorin Variation\", 'Sicilian Defense',\n",
              "       \"Queen's Pawn Game: Mason Attack\", 'Scotch Game',\n",
              "       'French Defense: Knight Variation', 'Philidor Defense #3'],\n",
              "      dtype=object)"
            ]
          },
          "execution_count": 172,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfgraphwhite.opening_name.unique()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 192,
      "id": "e263c714-bd0c-4dce-88e5-133000d44275",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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ASVnIiIiIiIi6YCSMxERERERkXRAyZmIiIiIiEg6oIdQi4i8Zlz67nzqc1JERETk9aORMxERERERkXRAyZmIiIiIiEg6oORMREREREQkHVByJiIiIiIikg4oORMREREREUkHlJyJiIiIiIikA0rORERERERE0gE950xE5DVTa2otLG3041tERP73tn2x7VWH8EbTyJmIiIiIiEg6oORMREREREQkHVByJiIiIiIikg4oORMREREREUkHlJyJiIiIiIikA0rORERERERE0gElZyIiIiIiIumAkjMREREREZF0QMmZiIiIiIhIOqDkTEReO76+vnz55ZevOgwRERGRF0rJmYik6MKFC3Tq1In8+fNjZWWFs7Mzfn5+7Nix44Wdw9XVlfHjx7+w9p4kLi6OgQMHUrx4cWxsbMiWLRvly5dn9OjR/PPPP/+TGERERERSYvmqAxCR9Ktp06bcu3eP0NBQ3Nzc+Pvvv1m/fj1Xrlx51aGl2ZUrV3j33XeJi4tj6NChlC1blkyZMvHnn38yd+5c5s6dy+eff/6qwxQREZG3mEbORCRZV69eZevWrXzzzTdUr16dAgUKUKFCBfr160f9+vXN6nXs2JFcuXJhbW1NiRIlWLlypVG+ZMkSihcvjpWVFa6urowdO9Yo8/X15fTp03Tv3h2TyYTJZDLKtm3bho+PD7a2tmTJkgU/Pz+z0a34+Hh69+5N1qxZcXZ2JjAw8In9+frrr4mJiWHXrl18+umneHl5UbRoURo0aMDcuXP57LPPjLpz5syhXLlyODg44OzsTKtWrbhw4YJRHhYWhslkYvXq1ZQpUwYbGxvee+89Lly4wB9//IGnpyeZM2emZcuW3Lx50zguISGB0aNH4+bmho2NDaVKlWLx4sVpe2NERETkjaXkTESSZW9vj729PcuXL+fOnTvJ1omPj6du3bps376dOXPmcPjwYUaNGoWFhQUAERERNGvWjBYtWnDw4EECAwMZOHAgISEhACxdupR8+fIxZMgQYmNjiY2NBSAyMpIaNWpQvHhxduzYwdatW2nYsCEPHjwwzh0aGoqdnR27du1i9OjRDBkyhLVr16YY54IFC2jdujV58+ZNts6jieHdu3cZOnQo+/fvZ/ny5Zw8eZKAgIAkxwQGBjJp0iS2b9/OmTNnaNasGePHj2fu3Ln89ttvrF27lu+//96oP2DAAIKDg5kyZQqHDh2ie/futG7dmk2bNiUb0507d4iLizPbRERE5M1lSkhISHjVQYhI+rRkyRI6dOjArVu38Pb2xsfHhxYtWuDl5QXAmjVrqFu3LlFRUXh4eCQ53t/fn4sXL7JmzRpjX+/evfntt984dOgQ8PCesy+//NJsgY9WrVoRExPD1q1bk43L19eXBw8esGXLFmNfhQoVeO+99xg1alSS+n///TfOzs6MGzeO7t27G/vLli3L0aNHAWjYsCHz5s1L9nzh4eFUqFCBf//9F3t7e8LCwqhevTrr1q2jRo0aAIwaNYp+/foRHR2Nm5sbAJ07d+bUqVOsWrWKGzdukD17djZs2EClSpWMttu3b8/NmzeZO3dukvMGBgYSFBSUZH+FbypgaaNZ6SIi8r+37YttrzqE105cXByOjo5cu3aNzJkzP7GuRs5EJEVNmzbl3LlzrFixAj8/P8LCwvD29jZGviIjI8mXL1+yiRlAVFQUVapUMdtXpUoVjh8/bjYK9rjEkbMnSUwQE+XOndts6mFyHh0dA1i2bBmRkZH4+flx69YtY/++ffto1KgRBQoUwMHBAV9fXwBiYmJSjCFXrlzY2toaiVnivsSYDh8+zO3bt6lVq5YxKmlvb8+sWbOIjo5ONt5+/fpx7do1Yztz5swT+yciIiKvN/3pVUSeyNramlq1alGrVi0GDRpE+/btGTx4MAEBAdjY2Dzx2ISEhCQJUWoG65/WLkDGjBnNXptMJuLj45OtmyNHDpycnDhy5IjZ/vz58wPg4ODA1atXAbhx4wa1a9emdu3azJkzhxw5chATE4Ofnx93795NMQaTyfTEmBL//e2335JMrbSysko2bisrqxTLRERE5M2jkTMRSZNixYpx48YN4OHI0V9//cWxY8dSrPv41MTt27fj4eFh3JeWKVOmJKNoXl5erF+//oXFnCFDBpo1a8acOXM4e/bsE+seOXKES5cuMWrUKKpWrUrRokWfOiKXGsWKFcPKyoqYmBgKFy5strm4uDx3+yIiIvL6U3ImIsm6fPky7733HnPmzOHAgQOcPHmSRYsWMXr0aBo1agSAj48P1apVo2nTpqxdu5aTJ0/yxx9/sGrVKgC++uor1q9fz9ChQzl27BihoaFMmjSJnj17GudxdXVl8+bNnD17lkuXLgEPp/OFh4fz2WefceDAAY4cOcKUKVOM8mcxYsQI8ubNS8WKFZk5cyYHDhwgOjqaZcuWsWPHDiNZzJ8/P5kyZeL777/nxIkTrFixgqFDhz7zeRM5ODjQs2dPunfvTmhoKNHR0ezbt48ffviB0NDQ525fREREXn+a1igiybK3t6dixYp89913REdHc+/ePVxcXOjQoQNff/21UW/JkiX07NmTli1bcuPGDQoXLmwsyuHt7c3ChQsZNGgQQ4cOJXfu3AwZMsRs5cMhQ4bQqVMnChUqxJ07d0hISMDDw4M1a9bw9ddfU6FCBWxsbKhYsSItW7Z85v5ky5aN3bt388033/Dtt99y8uRJMmTIgLu7O82bNzcWJMmRIwchISF8/fXXTJw4EW9vb8aMGcP777//zOdONHToUHLmzMnIkSM5ceIETk5OeHt7m11PEREReXtptUYRkddE4mpPWq1RREReFa3WmHZarVFEREREROQ1o+RMREREREQkHVByJiIiIiIikg4oORMREREREUkHlJyJiIiIiIikA0rORERERERE0gElZyIiIiIiIumAHpQjIvKaWdt57VOfkyIiIiKvH42ciYiIiIiIpANKzkRERERERNIBJWciIiIiIiLpgJIzERERERGRdEDJmYiIiIiISDqg5ExERERERCQdUHImIiIiIiKSDug5ZyIir5mtdepiZ6kf3yIibzufzZtedQjygmnkTEREREREJB1QciYiIiIiIpIOKDkTERERERFJB5SciYiIiIiIpANKzkRERERERNIBJWciIiIiIiLpgJIzERERERGRdEDJmYiIiIiISDqg5ExERERERCQdUHIm6VJISAhOTk6vNIbz589Tq1Yt7OzsXnksr0JYWBgmk4mrV6+m+pjAwEBKly790mJ6EU6dOoXJZCIyMjJdtCMiIiKSSMmZEBAQgMlkSrL9+eefrzq0NEn8sJy4OTg4ULx4cT7//HOOHz+e5va+++47YmNjiYyM5NixYy8h4pfHZDKxfPly4/W9e/do0aIFuXPn5sCBA6lqo3LlysTGxuLo6PhCY/P19eXLL798Yp2SJUvSvn37ZMvmzZtHxowZ+fvvv5/p/C4uLsTGxlKiRIlUHxMQEEDjxo2fux0RERGRJ1FyJgDUqVOH2NhYs61gwYJJ6t29e/cVRJc269atIzY2lv379zNixAiioqIoVaoU69evT1M70dHRlC1bFnd3d3LmzPmS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iIiIiIikA0rORERERERE0gElZyIiIiIiIumAkjMREREREZF0QMmZiIiIiIhIOqDkTEREREREJB3Qc85ERF4zI0eOxMrK6lWHISLy2ggMDHzVIYikikbORERERERE0gElZyIiIiIiIumAkjMREREREZF0QMmZiIiIiIhIOqDkTEREREREJB1QciYiIiIiIpIOKDkTERERERFJB5SciYiIiIiIpANKzkRERERERNIBJWciIv9Dvr6+fPnll686DBEREUmHlJy95s6cOUO7du3IkycPmTJlokCBAnTr1o3Lly+/6tCSFRYWhqur6zMf7+rqislkwmQyYWtrS4kSJZg2bdqLC/AF2bdvH82bNyd37txYWVlRoEABGjRowK+//kpCQsKrDi9VAgMDMZlM1KlTJ0nZ6NGjMZlM+Pr6/u8De0a1a9fGwsKCnTt3JikzmUwsX77cbF9gYCClS5f+3wQnIiIigpKz19qJEycoV64cx44dY968efz5559MnTqV9evXU6lSJa5cufKqQ3wphgwZQmxsLAcOHKBx48Z07tyZBQsWvOqwDL/88gvvvPMO169fJzQ0lMOHD7No0SIaN27MgAEDuHbt2qsOMdVy587Nxo0b+euvv8z2BwcHkz9//lcUVdrFxMSwY8cOunTpwowZM151OCIiIiLJUnL2Gvv888/JlCkTa9aswcfHh/z581O3bl3WrVvH2bNn6d+/v1E3uZEBJycnQkJCjNdnz56lefPmZMmShWzZstGoUSNOnTpldkxwcDCenp5YW1tTtGhRJk+ebJSdOnUKk8nE0qVLqV69Ora2tpQqVYodO3ak2If9+/dTvXp1HBwcyJw5M2XLlmXPnj1P7LeDgwPOzs4ULlyYYcOG4e7ubvStT58+eHh4YGtri5ubGwMHDuTevXsAXLt2DQsLCyIiIgBISEgga9aslC9f3mh73rx55M6d+5n7c+PGDdq1a0f9+vX57bffqF27NoUKFaJChQq0b9+e/fv34+joCMCDBw9o164dBQsWxMbGhiJFijBhwgSz9gICAmjcuDEjRowgV65cODk5ERQUxP379+nVqxdZs2YlX758zJw50+y41LyXqZEzZ05q165NaGiosW/79u1cunSJ+vXrm9UNDw+nVq1aZM+eHUdHR3x8fNi7d69ZncDAQPLnz4+VlRV58uSha9euRtk///xDmzZt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OqDkTEREREREJB1QciYiIiIiIpIOKDkTERERERFJB5SciYiIiIiIpAOWrzoAERFJm6XLqmNra/GqwxAReWWafbT7VYcg8lJo5ExERERERCQdUHImIiIiIiKSDig5ExERERERSQeUnImIiIiIiKQDSs5ERERERETSASVnIiIiIiIi6YCSMxERERERkXRAyZmIiIiIiEg6oORMREREREQkHVByJiIiIiIikg4oORN5A7i6ujJ+/PgUy0+dOoXJZCIyMvKlx/K/PNezCAkJwcnJKd20IyIiIpJIydkLcObMGdq1a0eePHnIlCkTBQoUoFu3bly+fPlVh5assLAwXF1dn/l4V1dXTCYTJpMJW1tbSpQowbRp015cgC/Ivn37aN68Oblz58bKyooCBQrQoEEDfv31VxISEl51eKkWFxdH//79KVq0KNbW1jg7O1OzZk2WLl2a6n64uLgQGxtLiRIlXnK0z3+uv//+m4wZMzJnzpxkyzt16oSXl9czx9e8eXOOHTuWpmOSS36fpR0RERGRJ1Fy9pxOnDhBuXLlOHbsGPPmzePPP/9k6tSprF+/nkqVKnHlypVXHeJLMWTIEGJjYzlw4ACNGzemc+fOLFiw4FWHZfjll1945513uH79OqGhoRw+fJhFixbRuHFjBgwYwLVr1151iKly9epVKleuzKxZs+jXrx979+5l8+bNNG/enN69e6e6HxYWFjg7O2NpaflS47179+5znytXrlzUr1+f4ODgJGW3bt1i/vz5tGvX7pnavnfvHjY2NuTMmfOZjn/Ui2pHREREJJGSs+f0+eefkylTJtasWYOPjw/58+enbt26rFu3jrNnz9K/f3+jrslkYvny5WbHOzk5ERISYrw+e/YszZs3J0uWLGTLlo1GjRpx6tQps2OCg4Px9PTE2tqaokWLMnnyZKMscUrZ0qVLqV69Ora2tpQqVYodO3ak2If9+/dTvXp1HBwcyJw5M2XLlmXPnj1P7LeDgwPOzs4ULlyYYcOG4e7ubvStT58+eHh4YGtri5ubGwMHDuTevXsAXLt2DQsLCyIiIgBISEgga9aslC9f3mh73rx55M6d+5n7c+PGDdq1a0f9+vX57bffqF27NoUKFaJChQq0b9+e/fv34+joCMCDBw9o164dBQsWxMbGhiJFijBhwgSz9gICAmjcuDEjRowgV65cODk5ERQU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9cIfNhAREdGbQybqus471VlYWBjWrFmDAwcOoEePHo0dDlGdfPHFF9i8eTNXBFRixcXFMDQ0RNQ2V+jpvZ6ppUREymjM6OpXuSVSRpW/v+/cuVPr7S685+wV+Pzzz2FtbY3jx4+je/fuCqv8ESmLjRs3olu3bjAxMcGxY8cQERGBgICAxg6LiIiI6I3F4uwVqVy4gEhZ5ebmYsmSJbh58ybatm2LOXPmYMGCBY0dFhEREdEbi8UZ0Rtq7dq1WLt2bWOHQURERET/H+fbERERERERKQEWZ0REREREREqAxRkREREREZESYHFGRERERESkBLggCBGRivF573Ct35NCREREqocjZ0REREREREqAxRkREREREZESYHFGRERERESkBFicERERERERKQEWZ0REREREREqAxRkREREREZES4FL6REQqpufug5Dr6Td2GEREr9zpUd6NHQLRa8WRMyIiIiIiIiXA4oyIiIiIiEgJsDgjIiIiIiJSAizOiIiIiIiIlACLMyIiIiIiIiXA4oyIiIiIiEgJsDgjIiIiIiJSAizOiIiIiIiIlACLMyIiIiIiIiXA4oyIiIiIiEgJsDgjopfO09MTgYGBjR3Gc+3evRt2dnaQy+VKHysRERG9GVicEakBPz8/jBw5skp7QkICZDIZbt++/VrjiYmJweLFi6Vta2trrFu37oWOuWjRIshkMshkMmhqasLU1BTu7u5Yt24dHj58WO/j+fv7Y9SoUbhy5YpCrERERESNhcUZET3Xo0eP6v0cY2NjGBgYvPRYOnTogIKCAuTn5+Pw4cMYPXo0li1bhp49e6KkpKTOx7l79y6Kiorg7e0NCwuLVxIrERERUX2xOCN6w/z888/o0KEDdHR0YG1tjdWrVyvst7a2xpIlS+Dn5wdDQ0P84x//wPvvv48ZM2ZIfQIDAyGTyXDu3DkAwJMnT2BgYIDY2FgAitMaPT09cfnyZQQFBUkjX5XtldtPPy5dulRj7JqamjAzM4OFhQU6duyIGTNmIDExEWfPnsWKFSukfo8ePcK8efPQunVr6Ovro3v37khISABQMZpYWYx5eXlBJpNJ+5KTk+Hu7g5dXV1YWlpi5syZKC0tVXhtli5dismTJ8PAwABt27bFli1bFM4bEBAAc3NzNGnSBNbW1li2bJm0/86dO/j000/RsmVLNGvWDF5eXjh9+nSN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",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.countplot(y='opening_name', \\\n",
        "              data = dfgraphwhite, \\\n",
        "              orient='h', \\\n",
        "              order=dfgraphwhite['opening_name'].value_counts().index)\n",
        "plt.ylabel('Opening name')\n",
        "plt.xlabel('Count')\n",
        "plt.show()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 193,
      "id": "a789cc4e-6310-4021-93b1-4a771aad6eba",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "Scandinavian Defense: Mieses-Kotroc Variation                        0.016398\n",
              "Sicilian Defense                                                     0.014899\n",
              "Scotch Game                                                          0.014499\n",
              "French Defense: Knight Variation                                     0.013499\n",
              "Philidor Defense #3                                                  0.012699\n",
              "                                                                       ...   \n",
              "King's Gambit Accepted |  Bishop's Gambit |  Bogoljubov Variation    0.000100\n",
              "Ruy Lopez: Noah's Ark Trap                                           0.000100\n",
              "Ruy Lopez: Closed Variations |  Martinez Variation                   0.000100\n",
              "Italian Game: Scotch Gambit |  de Riviere Defense                    0.000100\n",
              "Lion Defense: Anti-Philidor |  Lion's Cave                           0.000100\n",
              "Name: opening_name, Length: 1181, dtype: float64"
            ]
          },
          "execution_count": 193,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfwhite.opening_name.value_counts(normalize=True)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "3be62cae-cede-428b-9253-40ca3b5dc7ee",
      "metadata": {
        
      },
      "source": [
        "For the graph, we used an auxiliar dataframe. For that auxiliar dataframe (dfgraphwhite) we just added the top 10 most common **Opening Names**. For percentage, we used a table that hold all opening names. As we can see, **Scandinavian Defense: Mieses-Kotroc Variation** is the most common way to open a winning match with white pieces (1.63% out of 10001 games)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "73227b8e-e245-48fc-bf11-9c50ba0ed23d",
      "metadata": {
        
      },
      "source": [
        "### Black"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 196,
      "id": "a7b26e75-0743-4004-9ad3-cc0942f9ceb2",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "9107"
            ]
          },
          "execution_count": 196,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "len(dfblack.opening_name)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 197,
      "id": "3997f074-5198-4650-8459-149c9294a9a9",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "Van't Kruijs Opening                     226\n",
              "Sicilian Defense                         194\n",
              "Sicilian Defense: Bowdler Attack         164\n",
              "Scandinavian Defense                     123\n",
              "French Defense: Knight Variation         121\n",
              "Scotch Game                              115\n",
              "Queen's Pawn Game: Chigorin Variation    109\n",
              "Queen's Pawn Game: Mason Attack          103\n",
              "Indian Game                              100\n",
              "Philidor Defense #2                       96\n",
              "Name: opening_name, dtype: int64"
            ]
          },
          "execution_count": 197,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfblack.opening_name.value_counts().head(10)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 199,
      "id": "4f13698a-15a8-4e16-a393-06cbb47898a6",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfgraphblack = dfblack[dfblack.isin([\"Van't Kruijs Opening\", \\\n",
        "                                    'Sicilian Defense', \\\n",
        "                                    'Sicilian Defense: Bowdler Attack', \\\n",
        "                                    'Scandinavian Defense', \\\n",
        "                                    'French Defense: Knight Variation', \\\n",
        "                                    'Scotch Game', \\\n",
        "                                    \"Queen's Pawn Game: Chigorin Variation\", \\\n",
        "                                    \"Queen's Pawn Game: Mason Attack\", \\\n",
        "                                    'Indian Game', \\\n",
        "                                    'Philidor Defense #2'])]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 200,
      "id": "437e9a2a-a5b8-4ff2-9a19-2ed4596b502d",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "array([nan, 'French Defense: Knight Variation',\n",
              "       \"Queen's Pawn Game: Chigorin Variation\", 'Indian Game',\n",
              "       'Scandinavian Defense', 'Scotch Game',\n",
              "       \"Queen's Pawn Game: Mason Attack\", 'Sicilian Defense',\n",
              "       'Philidor Defense #2', \"Van't Kruijs Opening\",\n",
              "       'Sicilian Defense: Bowdler Attack'], dtype=object)"
            ]
          },
          "execution_count": 200,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfgraphblack.opening_name.unique()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 201,
      "id": "42f5fb80-8936-47a5-a3aa-f0677c08e3fd",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAzUAAAGwCAYAAACQDkuuAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/bCgiHAAAACXBIWXMAAA9hAAAPYQGoP6dpAACRPUlEQVR4nOzdd3yP1///8cdbkCGDxIgRgiQSYoVSfEjMmLVqRjU1SlFK7RGJXZuqUtokqi1aqqqtFWLFiBC0QjRGWo1aJRVb8vvDL++vtyQkRgnP++123Xhf51znep3rHfJ+vc+5zmVISUlJQUREREREJJvK8bwDEBEREREReRJKakREREREJFtTUiMiIiIiItmakhoREREREcnWlNSIiIiIiEi2pqRGRERERESyNSU1IiIiIiKSreV83gGIiPwXkpOT+euvv7CxscFgMDzvcERERCQTUlJS+PfffylSpAg5cmQ8HqOkRkReCX/99RdOTk7POwwRERF5DH/88QfFihXLsFxJjYi8EmxsbIB7/yna2to+52hEREQkMxITE3FycjL+Hs+IkhoReSWkTjmztbVVUiMiIpLNPGrquBYKEBERERGRbE0jNSLySqkz+hvMzC2fdxgiIiIvjahpXZ93CBqpERERERGR7E1JjYiIiIiIZGtKakREREREJFtTUiMiIiIiItmakhoREREREcnWlNSIiIiIiEi2pqRGRERERESyNSU1IiIiIiKSrSmpERERERGRbE1JjcgLxt/fn1atWmX4OrsLDw/HYDBw+fLl5x2KiIiIvCSU1MgLp0WLFjRo0CDdsl27dmEwGNi/f/9TOZezszPh4eEAnDp1CoPBQHR09EOPSa/ev//+i4+PD+7u7vzxxx9PFNOcOXMICQnJ8PXjCA0NpVq1auTJkwcbGxvq1KnD2rVrn6jNx1WzZk0SEhKws7N7LucXERGRl4+SGnnhdO/enc2bN3P69Ok0ZV988QWVKlXCy8vrOUSWvvPnz1O3bl2uXr3Kjh07cHJySrfe7du3M9WenZ0defPmzfB1Vg0ePJhevXrRvn17Dh48yN69e6lduzYtW7Zk3rx5j93u48qdOzeOjo4YDIb//NwiIiLyclJSIy+c5s2bU7BgwTSjE9euXWP58uV0796dixcv0qlTJ4oVK4aVlRXly5fnm2++Manv4+ND//79GTp0KPb29jg6OhIYGJjheUuWLAlA5cqVMRgM+Pj4PDLWP/74g9q1a2NjY8OWLVvInz8/8H+jOStWrMDHxwcLCwuWLl1KYGAglSpVMmlj9uzZODs7G18/avrZd999R/ny5bG0tMTBwYEGDRqQlJSUbny7d+9mxowZTJs2jcGDB+Pi4oKHhwcTJ07kgw8+YNCgQcaRpZCQEPLmzcvq1atxc3PDwsKChg0bphl5+vHHH6lSpQoWFhaUKlWKoKAg7ty5Yyw3GAwsXryY1q1bY2VlhaurK2vWrDGWPzj9LPW869evx8PDA2traxo3bkxCQoLxmDt37tC/f3/y5s2Lg4MDw4YN4+23336ppuWJiIjI41NSIy+cnDlz0rVrV0JCQkhJSTHu//bbb7l16xZ+fn7cuHGDKlWqsHbtWn799Vfeffdd3nrrLfbs2WPSVmhoKHny5GHPnj1MnTqVcePGsXHjxnTPu3fvXgA2bdpEQkICq1atemicx44do1atWri7u7Nu3TpsbGzS1Bk2bBj9+/cnJiYGX1/frF6KNBISEujUqRPdunUjJiaG8PBw2rRpY3Kd7vfNN99gbW1Nr1690pR9+OGH3L59m5UrVxr3Xbt2jYkTJxIaGsrOnTtJTEykY8eOxvL169fTpUsX+vfvz5EjR1i4cCEhISFMnDjRpO2goCDat2/PoUOHaNq0KX5+fly6dCnDfl27do3p06fz5Zdfsm3bNuLj4xk8eLCx/KOPPuKrr74iODjYGNfq1asfeq1u3rxJYmKiySYiIiIvJyU18kLq1q0bp06dMt7vAvemnrVp04Z8+fJRtGhRBg8eTKVKlShVqhTvv/8+vr6+fPvttybtVKhQgbFjx+Lq6krXrl2pWrUqYWFhxvJTp04ZR2QKFCgAgIODA46Ojtjb2z80xq5du1K6dGlWrlyJubl5unU++OAD2rRpQ8mSJSlSpMhjXAlTCQkJ3LlzhzZt2uDs7Ez58uXp06cP1tbW6daPjY2ldOnS5M6dO01ZkSJFsLOzIzY21rjv9u3bzJs3jxo1alClShVCQ0OJiIgwJnwTJ05k+PDhvP3225QqVYqGDRsyfvx4Fi5caNK2v78/nTp1wsXFhUmTJpGUlGRsIz23b99mwYIFVK1aFS8vL/r162fyPn388ceMGDGC1q1b4+7uzrx58x45JW/y5MnY2dkZt4ymBYqIiEj2p6RGXkju7u7UrFmTL774AoC4uDi2b99Ot27dALh79y4TJ06kQoUKODg4YG1tzYYNG4iPjzdpp0KFCiavCxcuzLlz555KjC1btmTHjh0mIx0Pqlq16lM5V6qKFStSv359ypcvT7t27Vi0aBH//PPPY7eXkpJicm9Lzpw5TWJ2d3cnb968xMTEABAVFcW4ceOwtrY2bj179iQhIYFr164Zj7v/uqcuTvCw625lZUXp0qWNr+9/n65cucLff/9NtWrVjOVmZmZUqVLloX0bMWIEV65cMW5PuoCDiIiIvLiU1MgLq3v37qxcuZLExESCg4MpUaIE9evXB2DGjBnMmjWLoUOHsnnzZqKjo/H19eXWrVsmbeTKlcvktcFgIDk5+anEN3LkSMaOHYufnx/Lly9Pt06ePHlMXufIkSPNVLHMLiAA9z7Mb9y4kV9++YWyZcvy8ccfU6ZMGU6ePJlufTc3N+Li4tJcF4C//vqLxMREXF1dTfandwN/6r7k5GSCgoKIjo42bocPH+b48eNYWFgY62f1uqdX/8Hr9GBcGU25S2Vubo6tra3JJiIiIi8nJTXywmrfvj1mZmZ8/fXXhIaG8s477xg/2G7fvp2WLVvSpUsXKlasSKlSpTh+/PgTnS91itbdu3czfczo0aMZP348fn5+aRYqSE+BAgU4e/asyQfyRy0h/SCDwUCtWrUICgriwIED5M6dm++//z7duh07duTq1atppocBTJ8+nVy5ctG2bVvjvjt37rBv3z7j62PHjnH58mXc3d0B8PLy4tixY7i4uKTZcuR4Nv+d2NnZUahQIZPpa3fv3uXAgQPP5HwiIiKS/eR83gGIZMTa2poOHTowcuRIrly5gr+/v7HMxcWFlStXEhERQb58+Zg5cyZnz57Fw8Pjsc9XsGBBLC0tWbduHcWKFcPCwiJTz1IZPnw4ZmZmvPXWWyQnJ+Pn55dhXR8fH86fP8/UqVN58803WbduHb/88kumRxH27NlDWFgYjRo1omDBguzZs4fz589n2O8aNWowYMAAhgwZwq1bt2jVqhW3b99m6dKlzJkzh9mzZ5vca5IrVy7ef/995s6dS65cuejXrx+vv/66cepXQEAAzZs3x8nJiXbt2pEjRw4OHTrE4cOHmTBhQqb68Djef/99Jk+ejIuLC+7u7nz88cf8888/WhZaREREAI3UyAuue/fu/PPPPzRo0IDixYsb948ZMwYvLy98fX3x8fHB0dHxiZf3zZkzJ3PnzmXhwoUUKVKEli1bZvrYIUOGMHXqVN5++22+/PLLDOt5eHgwf/58PvnkEypWrMjevXtNVvl6FFtbW7Zt20bTpk1xc3Nj9OjRzJgxgyZNmmR4zOzZs5k/fz7Lli2jfPnyVKlSha1bt7J69Wref/99k7pWVlYMGzaMzp07U6NGDSwtLVm2bJmx3NfXl7Vr17Jx40Zee+01Xn/9dWbOnEmJEiUy3YfHMWzYMDp16kTXrl2pUaMG1tbW+Pr6mkx5ExERkVeXIeVRE9NF5Lnq1KkTZmZmLF269JmeJyQkhA8++MD4/JgXWXJyMh4eHrRv357x48dn6pjExETs7Oyo+P4CzMwtn3GEIiIir46oaV2fWdupv7+vXLny0JktGqkReUHduXOHI0eOsGvXLsqVK/e8w3muTp8+zaJFi4iNjeXw4cO89957nDx5ks6dOz/v0EREROQFoKRG5AX166+/UrVqVcqVK0fv3r2fdzjPVY4cOQgJCeG1116jVq1aHD58mE2bNj3RPVQiIiLy8tD0MxF5JWj6mYiIyLOh6WciIiIiIiJPSEmNiIiIiIhka0pqREREREQkW1NSIyIiIiIi2VrO5x2AiMh/aduETg+90VBERESyH43UiIiIiIhItqakRkREREREsjUlNSIiIiIikq0pqRERERERkWxNSY2IiIiIiGRrSmpERERERCRbU1IjIiIiIiLZmp5TIyKvlD+mvI6NhdnzDkNERLK54gGHn3cIch+N1IiIiIiISLampEZERERERLI1JTUiIiIiIpKtKakREREREZFsTUmNiIiIiIhka0pqREREREQkW1NSIyIiIiIi2ZqSGhERERERydaU1IiIiIiISLampEYkiwwGA6tXr85U3cDAQCpVqmR87e/vT6tWrYyvfXx8+OCDD55qfM/atWvXaNu2Lba2thgMBi5fvvy8QxIREZFXnJIakfucO3eOXr16Ubx4cczNzXF0dMTX15ddu3YZ6yQkJNCkSZNMtTd48GDCwsIyLF+1ahXjx49/4rgfxWAwGLc8efLg6uqKv78/UVFRWW4rNDSU7du3ExERQUJCAnZ2ds8gYhEREZHMy/m8AxB5kbRt25bbt28TGhpKqVKl+PvvvwkLC+PSpUvGOo6Ojpluz9raGmtr6wzL7e3tnyjerAgODqZx48bcuHGD2NhYPvvsM6pXr84XX3xB165dM91OXFwcHh4eeHp6PsNoRURERDJPIzUi/9/ly5fZsWMHH330EXXr1qVEiRJUq1aNESNG0KxZM2O9B6ef/fnnn3Ts2BF7e3vy5MlD1apV2bNnD5B2+tmDHpx+tnTpUqpWrYqNjQ2Ojo507tyZc+fOGcvDw8MxGAyEhYVRtWpVrKysqFmzJseOHXtk//LmzYujoyPOzs40atSI7777Dj8/P/r168c///xjrBcREUGdOnWwtLTEycmJ/v37k5SUZIx3xowZbNu2DYPBgI+PDwC3bt1i6NChFC1alDx58lC9enXCw8ONbYaEhJA3b17Wr1+Ph4cH1tbWNG7cmISEBJO+VatWjTx58pA3b15q1arF6dOnjeU//vgjVapUwcLCglKlShEUFMSdO3ce2W8RERF5+SmpEfn/UkdVVq9ezc2bNzN1zNWrV/H29uavv/5izZo1HDx4kKFDh5KcnPxYMdy6dYvx48dz8OBBVq9ezcmTJ/H3909Tb9SoUcyYMYN9+/aRM2dOunXr9ljnGzhwIP/++y8bN24E4PDhw/j6+tKmTRsOHTrE8uXL2bFjB/369QPuTZfr2bMnNWrUICEhgVWrVgHwzjvvsHPnTpYtW8ahQ4do164djRs35vjx48ZzXbt2jenTp/Pll1+ybds24uPjGTx4MAB37tyhVatWeHt7c+jQIXbt2sW7776LwWAAYP369XTp0oX+/ftz5MgRFi5cSEhICBMnTsywbzdv3iQxMdFkExERkZeTpp+J/H85c+YkJCSEnj17smDBAry8vPD29qZjx45UqFAh3WO+/vprzp8/T2RkpHEqmYuLy2PHcH9yUqpUKebOnUu1atW4evWqyTS2iRMn4u3tDcDw4cNp1qwZN27cwMLCIkvnc3d3B+DUqVMATJs2jc6dOxtHj1xdXZk7dy7e3t58+umn2NvbY2VlRe7cuY3T8OLi4vjmm2/4888/KVKkCHDvXqJ169YRHBzMpEmTALh9+zYLFiygdOnSAPTr149x48YBkJiYyJUrV2jevLmx3MPDw6S/w4cP5+233zZem/HjxzN06FDGjh2bbt8mT55MUFBQlq6HiIiIZE8aqRG5T9u2bY2jLr6+voSHh+Pl5UVISEi69aOjo6lcufJTuzfmwIEDtGzZkhIlSmBjY2Oc3hUfH29S7/4kq3DhwgAm09QyKyUlBcA4IhIVFUVISIhx1Mra2hpfX1+Sk5M5efJkum3s37+flJQU3NzcTI7bunUrcXFxxnpWVlbGhCU17tSY7e3t8ff3x9fXlxYtWjBnzhyTqWlRUVGMGzfOpP2ePXuSkJDAtWvX0o1rxIgRXLlyxbj98ccfWb4+IiIikj1opEbkARYWFjRs2JCGDRsSEBBAjx49GDt2bLrTwCwtLZ/aeZOSkmjUqBGNGjVi6dKlFChQgPj4eHx9fbl165ZJ3Vy5chn/npqQPM6Ut5iYGABKlixpbKNXr170798/Td3ixYun20ZycjJmZmZERUVhZmZmUnb/6NL9MafGnZpUwb2FDPr378+6detYvnw5o0ePZuPGjbz++uskJycTFBREmzZt0pw/o9Epc3NzzM3N0y0TERGRl4uSGpFHKFu2bIbPpalQoQKLFy/m0qVLTzxac/ToUS5cuMCUKVNwcnICYN++fU/U5qPMnj0bW1tbGjRoAICXlxe//fZblqbQVa5cmbt373Lu3Dlq1679RPFUrlyZypUrM2LECGrUqMHXX3/N66+/jpeXF8eOHXuiqX0iIiLy8tL0M5H/7+LFi9SrV4+lS5dy6NAhTp48ybfffsvUqVNp2bJlusd06tQJR0dHWrVqxc6dOzlx4gQrV640ea5NZhUvXpzcuXPz8ccfc+LECdasWfNUn2Fz+fJlzp49y+nTp9m4cSNvvvkmX3/9NZ9++il58+YFYNiwYezatYu+ffsSHR3N8ePHWbNmDe+//36G7bq5ueHn50fXrl1ZtWoVJ0+eJDIyko8++oiff/45U7GdPHmSESNGsGvXLk6fPs2GDRuIjY013lcTEBDAkiVLCAwM5LfffiMmJsY4miMiIiKikRqR/8/a2prq1asza9Ys4uLiuH37Nk5OTvTs2ZORI0eme0zu3LnZsGEDH374IU2bNuXOnTuULVuWTz75JMvnL1CgACEhIYwcOZK5c+fi5eXF9OnTeeONN560a8C9Fcrg3nStokWL8r///Y+9e/fi5eVlrFOhQgW2bt3KqFGjqF27NikpKZQuXZoOHTo8tO3g4GAmTJjAhx9+yJkzZ3BwcKBGjRo0bdo0U7FZWVlx9OhRQkNDuXjxIoULF6Zfv3706tULAF9fX9auXcu4ceOYOnUquXLlwt3dnR49ejzm1RAREZGXiSHl/kntIiIvqcTEROzs7Ph1hAc2FmaPPkBEROQhigccft4hvBJSf39fuXIFW1vbDOtp+pmIiIiIiGRrSmpERERERCRbU1IjIiIiIiLZmpIaERERERHJ1pTUiIiIiIhItqakRkREREREsjUlNSIiIiIikq3p4Zsi8kpxGr77oevci4iISPajkRoREREREcnWlNSIiIiIiEi2pqRGRERERESyNSU1IiIiIiKSrSmpERERERGRbE1JjYiIiIiIZGtKakREREREJFvTc2pE5JXScEFDclrqvz4Rkadl5/s7n3cIIhqpERERERGR7E1JjYiIiIiIZGtKakREREREJFtTUiMiIiIiItmakhoREREREcnWlNSIiIiIiEi2pqRGRERERESyNSU1IiIiIiKSrSmpERERERGRbE1JzX/IYDCwevXqTNUNDAykUqVKxtf+/v60atXK+NrHx4cPPvjgqcb3rF27do22bdtia2uLwWDg8uXLzzukbOdRPxcvs1epryIiIpI1SmqeknPnztGrVy+KFy+Oubk5jo6O+Pr6smvXLmOdhIQEmjRpkqn2Bg8eTFhYWIblq1atYvz48U8c96MYDAbjlidPHlxdXfH39ycqKirLbYWGhrJ9+3YiIiJISEjAzs7uGUT8bDg7Oxuvg5mZGUWKFKF79+78888/zzu0Z+7dd9/FzMyMZcuWpSlzdnZm9uzZJvtCQkLImzfvfxOciIiICEpqnpq2bdty8OBBQkNDiY2NZc2aNfj4+HDp0iVjHUdHR8zNzTPVnrW1NQ4ODhmW29vbY2Nj88RxZ0ZwcDAJCQn89ttvfPLJJ1y9epXq1auzZMmSLLUTFxeHh4cHnp6eODo6YjAYnlHEz8a4ceNISEggPj6er776im3bttG/f//nHdYTSUlJ4c6dOxmWX7t2jeXLlzNkyBA+//zz/zAyERERkcxTUvMUXL58mR07dvDRRx9Rt25dSpQoQbVq1RgxYgTNmjUz1ntw+tmff/5Jx44dsbe3J0+ePFStWpU9e/YAaacZPejB6WdLly6latWq2NjY4OjoSOfOnTl37pyxPDw8HIPBQFhYGFWrVsXKyoqaNWty7NixR/Yvb968ODo64uzsTKNGjfjuu+/w8/OjX79+JiMVERER1KlTB0tLS5ycnOjfvz9JSUnGeGfMmMG2bdswGAz4+PgAcOvWLYYOHUrRokXJkycP1atXJzw83Nhm6rf+69evx8PDA2traxo3bkxCQoJJ36pVq0aePHnImzcvtWrV4vTp08byH3/8kSpVqmBhYUGpUqUICgp66Af5jKRe26JFi1K3bl26du3K/v37TeqsXLmScuXKYW5ujrOzMzNmzDCWffzxx5QvX974evXq1RgMBj755BPjPl9fX0aMGGF8PWXKFAoVKoSNjQ3du3fnxo0bD40xJSWFqVOnUqpUKSwtLalYsSLfffedybUyGAysX7+eqlWrYm5uzvbt2zNs79tvv6Vs2bKMGDGCnTt3curUKWOZj48Pp0+fZuDAgcZRrPDwcN555x2uXLli3BcYGAg8+mcU4LfffqNZs2bY2tpiY2ND7dq1iYuLSze2qKgoChYsyMSJEx96TUREROTlp6TmKbC2tsba2prVq1dz8+bNTB1z9epVvL29+euvv1izZg0HDx5k6NChJCcnP1YMt27dYvz48Rw8eJDVq1dz8uRJ/P3909QbNWoUM2bMYN++feTMmZNu3bo91vkGDhzIv//+y8aNGwE4fPgwvr6+tGnThkOHDrF8+XJ27NhBv379gHvT5Xr27EmNGjVISEhg1apVALzzzjvs3LmTZcuWcejQIdq1a0fjxo05fvy48VzXrl1j+vTpfPnll2zbto34+HgGDx4MwJ07d2jVqhXe3t4cOnSIXbt28e677xpHgdavX0+XLl3o378/R44cYeHChYSEhJh8EPb39zcmWZl15swZ1q5dS/Xq1Y37oqKiaN++PR07duTw4cMEBgYyZswYQkJCgHtJwG+//caFCxcA2Lp1K/nz52fr1q3GvkRERODt7Q3AihUrGDt2LBMnTmTfvn0ULlyY+fPnPzSu0aNHExwczKeffspvv/3GwIED6dKli/EcqYYOHcrkyZOJiYmhQoUKGbb3+eef06VLF+zs7GjatCnBwcHGslWrVlGsWDHjCFZCQgI1a9Zk9uzZ2NraGvelvleP+hk9c+YMderUwcLCgs2bNxMVFUW3bt3STUDDw8OpX78+QUFBjBo1Kt3Yb968SWJioskmIiIiL6eczzuAl0HOnDkJCQmhZ8+eLFiwAC8vL7y9venYsWOGHxi//vprzp8/T2RkJPb29gC4uLg8dgz3JyelSpVi7ty5VKtWjatXr2JtbW0smzhxovFD8/Dhw2nWrBk3btzAwsIiS+dzd3cHMH5zP23aNDp37mwcPXJ1dWXu3Ll4e3vz6aefYm9vj5WVFblz58bR0RG4Nx3tm2++4c8//6RIkSLAvXuJ1q1bR3BwMJMmTQLg9u3bLFiwgNKlSwPQr18/xo0bB0BiYiJXrlyhefPmxnIPDw+T/g4fPpy3337beG3Gjx/P0KFDGTt2LACFCxfOVDI5bNgwRo8ezd27d7lx4wbVq1dn5syZxvKZM2dSv359xowZA4CbmxtHjhxh2rRp+Pv74+npiYODA1u3bqVt27aEh4fz4YcfMmvWLAAiIyO5ceMG//vf/wCYPXs23bp1o0ePHgBMmDCBTZs2ZThak5SUxMyZM9m8eTM1atQw9nfHjh0sXLjQ+L7Dval0DRs2fGh/jx8/zu7du40JaGpyOHbsWHLkyIG9vT1mZmbGkZdUdnZ2GAwGk33w6J/RTz75BDs7O5YtW0auXLmM1/BBP/zwA2+99RYLFy6kU6dOGcY/efJkgoKCHtpHEREReTlopOYpadu2rXHUxdfXl/DwcLy8vIzf0j8oOjqaypUrGxOaJ3XgwAFatmxJiRIlsLGxMY48xMfHm9S7P8kqXLgwQJopQJmRkpICYBwRiYqKIiQkxDhqZW1tja+vL8nJyZw8eTLdNvbv309KSgpubm4mx23dutVkypGVlZUxYUmNOzVme3t7/P398fX1pUWLFsyZM8dkalpUVBTjxo0zab9nz54kJCRw7do14N6H38zcHzRkyBCio6M5dOiQcRGHZs2acffuXQBiYmKoVauWyTG1atXi+PHj3L17F4PBQJ06dQgPD+fy5cv89ttv9O7dm7t37xITE2P8mUlNQmNiYozJSaoHX9/vyJEj3Lhxg4YNG5r0d8mSJWmmcFWtWvWR/f3888/x9fUlf/78ADRt2pSkpCQ2bdr0yGPT86if0ejoaGrXrm1MaNKzZ88e2rZtS2ho6EMTGoARI0Zw5coV4/bHH388VtwiIiLy4tNIzVNkYWFBw4YNadiwIQEBAfTo0YOxY8emOw3M0tLyqZ03KSmJRo0a0ahRI5YuXUqBAgWIj4/H19eXW7dumdS9/wNjakLyOFPeYmJiAChZsqSxjV69eqV743zx4sXTbSM5ORkzMzOioqIwMzMzKbt/dOnBD7kGg8GYVMG9hQz69+/PunXrWL58OaNHj2bjxo28/vrrJCcnExQURJs2bdKcP6ujU/nz5zeOprm6ujJ79mxq1KjBli1baNCgASkpKWkWP7g/Trg3Be2zzz5j+/btVKxYkbx581KnTh22bt1KeHh4lqfB3S/1ffzpp58oWrSoSdmDC1TkyZPnoW3dvXuXJUuWcPbsWXLmzGmy//PPP6dRo0ZZii0zP6OZ+TdRunRpHBwc+OKLL2jWrBm5c+fOsK65uXmmF+YQERGR7E1JzTNUtmzZDJ9LU6FCBRYvXsylS5eeeLTm6NGjXLhwgSlTpuDk5ATAvn37nqjNR0m9b6JBgwYAeHl58dtvv2VpCl3lypW5e/cu586do3bt2k8UT+XKlalcuTIjRoygRo0afP3117z++ut4eXlx7NixJ5ral5HUROz69evAvfd7x44dJnUiIiJwc3Mz1vXx8WHAgAF89913xgTG29ubTZs2ERERwYABA4zHenh4sHv3brp27Wrct3v37gzjKVu2LObm5sTHx5tMNXscP//8M//++y8HDhwwSTiPHj2Kn58fFy9exMHBgdy5cxtHqlKlty8zP6MVKlQgNDSU27dvZzhakz9/flatWoWPjw8dOnRgxYoVDx3ZERERkVeDpp89BRcvXqRevXosXbqUQ4cOcfLkSb799lumTp1Ky5Yt0z2mU6dOODo60qpVK3bu3MmJEydYuXKlyXNtMqt48eLkzp2bjz/+mBMnTrBmzZqn+gyby5cvc/bsWU6fPs3GjRt58803+frrr/n000+NzyMZNmwYu3btom/fvkRHR3P8+HHWrFnD+++/n2G7bm5u+Pn50bVrV1atWsXJkyeJjIzko48+4ueff85UbCdPnmTEiBHs2rWL06dPs2HDBmJjY4331QQEBLBkyRICAwP57bffiImJMY7mpBoxYoRJ4pCRf//9l7Nnz5KQkMDevXsZMmQI+fPnp2bNmgB8+OGHhIWFMX78eGJjYwkNDWXevHnGG+UB4301X331lTGp8fHxYfXq1Vy/ft14Pw3AgAED+OKLL/jiiy+IjY1l7Nix/PbbbxnGZ2Njw+DBgxk4cCChoaHExcVx4MABPvnkE0JDQzN1PVN9/vnnNGvWjIoVK+Lp6Wnc2rZtS4ECBVi6dClw7zk127Zt48yZM8YFEJydnbl69SphYWFcuHCBa9euZepntF+/fiQmJtKxY0f27dvH8ePH+fLLL9Os0FewYEE2b97M0aNH6dSp02OtZCciIiIvFyU1T4G1tTXVq1dn1qxZ1KlTB09PT8aMGUPPnj2ZN29eusfkzp2bDRs2ULBgQZo2bUr58uWZMmVKmmlYmVGgQAFCQkKMy+9OmTKF6dOnP2m3jN555x0KFy6Mu7s77733HtbW1uzdu5fOnTsb61SoUIGtW7dy/PhxateuTeXKlRkzZozxvp2MBAcH07VrVz788EPKlCnDG2+8wZ49e4zf5j+KlZUVR48epW3btri5ufHuu+/Sr18/evXqBdxbInnt2rVs3LiR1157jddff52ZM2dSokQJYxupz555lICAAAoXLkyRIkVo3rw5efLkYePGjcbnCXl5ebFixQqWLVuGp6cnAQEBjBs3zmT6ocFgMI6ipI5OVahQATs7OypXroytra2xbocOHQgICGDYsGFUqVKF06dP89577z00xvHjxxMQEMDkyZPx8PDA19eXH3/80ThNMDP+/vtvfvrpJ9q2bZumzGAw0KZNG+Mza8aNG8epU6coXbo0BQoUAKBmzZr07t2bDh06UKBAAaZOnZqpn1EHBwc2b95sXBmwSpUqLFq0KN2RGEdHRzZv3szhw4fx8/NLMzIkIiIirxZDyoOT/kVEXkKJiYnY2dlR7aNq5LTUzFsRkadl5/s7n3cI8hJL/f195coVky9/H6SRGhERERERydaU1IiIiIiISLampEZERERERLI1JTUiIiIiIpKtKakREREREZFsTUmNiIiIiIhka0pqREREREQkW9PDGkTklbKx98aHrnMvIiIi2Y9GakREREREJFtTUiMiIiIiItmakhoREREREcnWlNSIiIiIiEi2pqRGRERERESyNSU1IiIiIiKSrSmpERERERGRbE3PqRGRV8qOxk3Ik1P/9Ym8iry3bX3eIYjIM6KRGhERERERydaU1IiIiIiISLampEZERERERLI1JTUiIiIiIpKtKakREREREZFsTUmNiIiIiIhka0pqREREREQkW1NSIyIiIiIi2ZqSGhERERERydaU1Mgry2AwsHr1agBOnTqFwWAgOjr6P43B39+fVq1a/afnfFIpKSm8++672NvbP5drJiIiIvIgJTXyTJ07d45evXpRvHhxzM3NcXR0xNfXl127dj3v0Ew4OTmRkJCAp6fnf3reOXPmEBIS8szP4+zsjMFgwGAwYGlpibOzM+3bt2fz5s1Zbmvd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yUcffYSnpydfffUVkydPNmnbzc2NDRs2cPDgQapVq0aNGjX44YcfjPf03K9MmTJs3ryZb775hg8//PCx+ikiIiIvD0OKJqYDMHbsWGbOnMmGDRuoUaPG8w5HRJ6yxMRE7OzsGD58uBYQEHnKAgMDn3cIIvKSSv39feXKlYfeHvDK3VOTkaCgIJydndmzZw/Vq1c3WTVLREREREReXEpq7vPOO+887xBERERERCSLNBwhIiIiIiLZmpIaERERERHJ1pTUiIiIiIhItqakRkREREREsjUlNSIiIiIikq3pOTUi8krI7Dr3IiIi8uLI7O9vjdSIiIiIiEi2pqRGRERERESyNSU1IiIiIiKSrSmpERERERGRbE1JjYiIiIiIZGtKakREREREJFvL+bwDEBH5L636vi5WVmbPOwyRbKl9u73POwQRkXRppEZERERERLI1JTUiIiIiIpKtKakREREREZFsTUmNiIiIiIhka0pqREREREQkW1NSIyIiIiIi2ZqSGhERERERydaU1IiIiIiISLampEZERERERLI1JTUiIiIiIpKtKakRyUYMBgOrV68G4NSpUxgMBqKjo59rTCIiIiLPm5Iakf+Av78/rVq1eqptOjk5kZCQgKen51NtNz23bt1i2rRpeHl5kSdPHuzs7KhYsSKjR4/mr7/+eubnFxEREXkYJTUi2ZSZmRmOjo7kzJnzmZ7n5s2bNGzYkEmTJuHv78+2bduIiopi6tSpXLx4kY8//viZnl9ERETkUZTUiDwHPj4+9O/fn6FDh2Jvb4+joyOBgYEmdY4fP06dOnWwsLCgbNmybNy40aT8welnd+/epXv37pQsWRJLS0vKlCnDnDlzTI5JHTGaPn06hQsXxsHBgb59+3L79u0MY501axY7duxg8+bN9O/fnypVquDi4oKvry+ffvopkyZNMtZdt24d//vf/8ibNy8ODg40b96cuLi4NDGvWLGC2rVrY2lpyWuvvUZsbCyRkZFUrVoVa2trGjduzPnz503iCA4OxsPDAwsLC9zd3Zk/f/5Dr/HNmzdJTEw02UREROTlpKRG5DkJDQ0lT5487Nmzh6lTpzJu3Dhj4pKcnEybNm0wMzNj9+7dLFiwgGHDhj20veTkZIoVK8aKFSs4cuQIAQEBjBw5khUrVpjU27JlC3FxcWzZsoXQ0FBCQkIICQnJsN1vvvmG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9u3b8/fff6c55xdffEGpUqUwNzcnJSUlwxgKFiyIo6Mjbm5udOzYkZ07d1KgQAHee+89k3rBwcF4eHhgYWGBu7s78+fPN5YZDAaioqIYN24cBoOBwMBAAM6cOUOHDh3Ily8fDg4OtGzZklOnThmPS52ON336dAoXLoyDgwN9+/bl9u3bxjrz58/H1dUVCwsLChUqxJtvvmksS0lJYerUqZQqVQpLS0sqVqzId999l2FfHxQREWFManbs2GH8e6rUpLZRo0aUKlWKN954g8GDB7Nq1aoM27x58yaJiYkmm4iIiLyclNRItmRpaWnygRtg1KhRzJgxg3379pEzZ066dev22O37+/tz6tQpNm/ezHfffcf8+fM5d+6csTwlJYVWrVpx6dIltm7dysaNG4mLi6NDhw4m7fz++++sWLGClStXEh0dneU+9u7dm507dxrPvej/tXf/QVXV+R/HnxdBkB9yZRUuKCAUiSK0CdriKkgBQulq7u64k7vRTDrjpDSkbttmBmOljWWzkz/TNiKnNnc2s6YfFqLgMo4b4vLV9Gbgj71swlD+ALIAkfP9w/GOV8Cg1Xu58HrMMMM5n88553Puew7c1z0/7tatLF++nOeffx6r1cqqVatYsWIFRUVFANTV1REXF8fSpUupq6tj2bJlfP/996SlpeHv78++ffsoLy/H39+frKwsh8u39u7dy4kTJ9i7dy9FRUW88cYbvPHGGwAcPHiQxx57jJUrV3L8+HF27dpFSkqKfdmnn36awsJCNm3axNGjR3n88cf5/e9/T1lZWbf7V15ejtlsxmw2849//IPly5djNpvZvHkzr7zyCmazmRdeeKHb5RsbG294hm316tUEBgbaf8LDw3v0uouIiIj70eVn4nY+//xz3n77be69916H+c8//zypqakAPPnkk9x///20tLTg4+PTq/V/9dVXfPLJJxw4cIC7774bgL/+9a+MHTvW3mf37t0cPnyYU6dO2d8sb9u2jbi4OCoqKpg4cSIAbW1tbNu2jREjRvykfY2NjQWu3FMSHBzMs88+y9q1a5kzZw4AUVFRHDt2jFdffZWcnBwsFguenp74+/tjsVgAeP311/Hw8OC1117DZDIBV872mM1mSktLyczMBGDYsGGsX7+eQYMGERsby/33309JSQkLFizAZrPh5+fHjBkzCAgIIDIykrvuuguA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",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.countplot(y='opening_name', \\\n",
        "              data = dfgraphblack, \\\n",
        "              orient='h', \\\n",
        "              order=dfgraphblack['opening_name'].value_counts().index)\n",
        "plt.ylabel('Opening name')\n",
        "plt.xlabel('Count')\n",
        "plt.show()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 202,
      "id": "ae0d37dd-d108-4313-8163-d7d98c841dba",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "Van't Kruijs Opening                                               0.024816\n",
              "Sicilian Defense                                                   0.021302\n",
              "Sicilian Defense: Bowdler Attack                                   0.018008\n",
              "Scandinavian Defense                                               0.013506\n",
              "French Defense: Knight Variation                                   0.013286\n",
              "                                                                     ...   \n",
              "Ruy Lopez: Closed Variations |  Yates Variation |  Short Attack    0.000110\n",
              "Queen's Gambit Accepted: Showalter Variation                       0.000110\n",
              "King's Gambit Accepted |  Greco Gambit                             0.000110\n",
              "Van Geet Opening: Dunst-Perrenet Gambit                            0.000110\n",
              "Pirc Defense: Austrian Attack |  Dragon Formation                  0.000110\n",
              "Name: opening_name, Length: 1145, dtype: float64"
            ]
          },
          "execution_count": 202,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfblack.opening_name.value_counts(normalize=True)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "033c98d4-f25e-42a6-8786-a12bc3e378a3",
      "metadata": {
        
      },
      "source": [
        "For black pieces we can see that the most common win strategy is **Van't Kruijs Opening**, having %2.48 of the total black piece wins (9107)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "c0a9e1f6-52ba-4682-afb3-a91934cd0b95",
      "metadata": {
        
      },
      "source": [
        "## Are most of the games rated?"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 209,
      "id": "07a925b1-e695-4d68-8eac-967ce1afd2d9",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "iVBORw0KGgoAAAANSUhEUgAAAZkAAAGFCAYAAAAvsY4uAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjcuMSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/bCgiHAAAACXBIWXMAAA9hAAAPYQGoP6dpAAAsHElEQVR4nO3dd3hUZd4+8HsmmWTSJgnpCYQUiCSEDiJIVRGQ3VUB2RX2BVR2beir/lTsK4Lu6ruwWGBFUUEBF1BRbBSXKj1ACKEGAqSQ3vvU3x/RKNKSyZzzzDnn/lzXXEoYZm4E58455znfR+dwOBwgIiKSgF50ACIiUi+WDBERSYYlQ0REkmHJEBGRZFgyREQkGZYMERFJhiVDRESSYckQEZFkWDJERCQZlgwREUmGJUNERJJhyRARkWRYMkREJBmWDBERSYYlQ0REkmHJEBGRZFgyREQkGZYMERFJhiVDRESSYckQEZFkWDJERCQZlgwREUmGJUNERJJhyRARkWRYMkREJBmWDBERSYYlQ0REkmHJEBGRZFgyREQkGZYMERFJhiVDRESSYckQEZFkWDJERCQZlgwREUmGJUNERJJhyRARkWRYMkREJBmWDBERSYYlQ0REkmHJEBGRZFgyREQkGZYMERFJhiVDRESSYckQEZFkWDJERCQZT9EBiNydze5AWV0TSmvMKK1tannUNlphtjlgsdlhsdlhttphsTngcDgAHaCDDgCg0wF6HeDvbUCgjwFBvs0Pk48BQT4/f80LgT4GeOh1gn+3RK7FkiFNs9sdyCmvx8miGpwtrUNJzS8l8nOpVNSbYXdIn0WnA/y9PBHoa0CovzfiQnwRH+qP+DA/JIT6IT7UD37e/F+WlEXncDhk+N+HSLy8inpkFdXiZFENTv30OF1ci0aLXXS0VgsP8EZ8qB8SwppLJz7Uv/nfQ/yg51EQuSGWDKnS6eIa7M4uR2ZeFU4V1+B0US1qmqyiY0kmwOiJPrHB6N+5+dE7Ngi+XjzqIfFYMqQK2SW12J1dht1nyrAnuxyltU2iIwnlqdchOcqEfp2D0T8uGP07d0BkoFF0LNIglgwp0vmyOuw+U4bd2WXYk12Gomptl0prxAT5oF/nYAxKDMFN3cIRYWLpkPRYMqQIjRYbtp8qwcZjRdh1uhQXqhpFR1I0nQ5IjQ7EzcnhuCU5AqkxgaIjkUqxZMht1TVZseVkMb7PLMTWE8WoM9tER1KtqEAjbk2JwG09ojAgrgMXEZDLsGTIrTRZbdhyohhfpV/A5hPFaLIqZ+WXWoQHeGN090jc1iMKA+NZONQ+LBkSzm53YE92Gb5Mz8f3mYWoaVTvKjCliTB544/9O+FP18ciOshHdBxSIJYMCVNa24SVe3Pw6b4cFPAai1vz0OswIikMU26IxYikcB7dUKuxZEh2mflV+HDnWXyTUQAzT4cpTkyQD/40oBP+eH0nhAdwhRpdHUuGZGG12fF9ZiGW7jqHA+crRMchF/DU6zAqJQKTB8ZiSJdQ6HQ8uqFLsWRIUuV1Zqzcex7L9+SgsJqnxNQqLsQX0wbH4e7rY2E0eIiOQ26EJUOSOFFYjSU7zuLrwxe4QkxDwgO88cDwREweyLKhZiwZcqlzpXWYt+kUvsm4AP7N0i6WDf2MJUMuUVjViDf/m4U1abmwyjEXnxSBZUMsGWqXynozFm09g2W7zvG0GF1RhKm5bHjNRntYMuSUuiYrPvjxLN7fnq3qEfrkWhEmbzw0ogumDIyFpwd3f9cClgy1SZPVhhV7crBo62mU1ppFxyGFui4iAK/c3h0DE0JERyGJsWSo1f57vAh/W3cUeRUNoqOQStzROxrPjUvmTZ0qxpKhayquacTsdcfw7ZEC0VFIhQKMnnj8liRMGxwHD46rUR2WDF2Rw+HAp/ty8Y/vj6OaQytJYslRJsy5vTv6x3UQHYVciCVDl3W6uBbPfXEE+86Vi45CGqLTAeP7dMSzt3VDqL+36DjkAiwZuojZasfCLafx761nYLZxSTKJYTJ64qkx3fDngbGciaZwLBlqse9sOZ79IgNnSupERyECAAzpEop/3tULkYFcGKBULBlCg9mGud8ew8p9ORwFQ24n0MeAuXek4ve9okVHISewZDTu2IVqPPLpQR69kNv7Q69ozLkjFYE+BtFRqA1YMhq2bNc5vPrdcW4cRooRE+SDt+7ujX6duQJNKVgyGlRZb8ZTn2Vg07Ei0VGI2sxTr8Pjo5Lw4PBEbgOtACwZjTmcW4mHVhxEfiXv2idlG9IlFP/6Y2+EBXCpsztjyWjIJ7vPYc43x7k0mVQj1N8bb9/dB4MSOQPNXbFkNKDebMWzXxzBV+kXREchcjmDhw5z70jFHwfEio5Cl8GSUbmcsnrM+Hg/ThXVio5CJKm/DI3Hs2OTeZ3GzbBkVCwjrxL3Lt3PkfykGaNSIvDmn3rD18tTdBT6CUtGpTafKMLMlYdQb7aJjkIkq5QoEz6Y3h9RgT6ioxBYMqq0cm8OXvwqEzY7/2hJm8IDvLFkWn/07BgkOormsWRU5v82nMDCLWdExyASzmjQY/6k3ritR5ToKJrGklEJi82Opz/LwNpD+aKjELkNnQ74f6OSMPOmrqKjaBZLRgVqGi14YPkB7DxdJjoKkVuaOqgzXrk9VXQMTWLJKFxhVSOmf7QPJwprREchcmt/viEWc25P5f40MmPJKFhueT3+9N4ejoghaqXJA2Px6h0sGjnpRQcg5xRUNWDyEhYMUVus3JuD59YeAb+3lg9LRoGKaxox+f29yC1nwRC11af7cjHr8wzYucRfFiwZhSmrbcKU9/fibCk3GSNy1uq0PDzNopEFS0ZBKuvNmLJkL7KKOYeMqL0+O5CHJz87zKKRGEtGIaobLZj6IVeREbnSFwfz8cTqdE7HkBBLRgHqmqy456P9yMirEh2FSHW+TL+AWZ9niI6hWiwZN9doseHepftx4HyF6ChEqvXZgTy8+UOW6BiqpLqS0el0V31Mnz5ddMRWs9js+MvHadh7tlx0FCLV+9cPp7D2UJ7oGKqjuk0XCgoKWv591apVeOmll3Dy5MmWr/n4XDz+22KxwGAwyJavLV766ih2ZJWKjkGkGbM+O4LoQB8MTOB2zq6iuiOZyMjIlkdgYCB0Ol3LjxsbGxEUFITVq1djxIgRMBqNWL58OV5++WX07t37otdZsGAB4uLiLvraRx99hOTkZBiNRnTr1g2LFi2S7PexbNc5fLovR7LXJ6JLmW123L/8ALJLuILTVVRXMq0xa9YsPProozh+/DhGjx7dql/z/vvv4/nnn8err76K48eP47XXXsOLL76IZcuWuTzfj1mlmPPNMZe/LhFdW2W9Bfcs3Y+y2ibRUVRBdafLWuOxxx7D+PHj2/Rr5syZg3nz5rX8uvj4eBw7dgyLFy/GtGnTXJbtbGkdHl55EFYuqSQS5nxZPf7ycRpW/uUGGA0eouMomiaPZPr379+m55eUlCA3Nxf33Xcf/P39Wx5z587FmTOu2yCsutGCGcv2o6rB4rLXJCLnHMypxP9bfZhzztpJk0cyfn5+F/1Yr9df8hfJYvnlg95utwNoPmU2cODAi57n4eGa73JsdgceWXkIZ0o4LobIXXx7pACd1vvimbHdREdRLE2WzG+FhYWhsLAQDoejZQR4enp6y89HREQgJiYG2dnZmDJliiQZXvvuOLadKpHktYnIee9uO4PkqADc3jtGdBRFYskAGDFiBEpKSvDGG29g4sSJWL9+Pb7//nuYTKaW57z88st49NFHYTKZMHbsWDQ1NSEtLQ0VFRV44okn2vX+q9Ny8cGPZ9v72yAiiTz3xRGkxgQiMcxfdBTF0eQ1md9KTk7GokWLsHDhQvTq1Qv79u3Dk08+edFzZsyYgSVLlmDp0qXo0aMHhg8fjqVLlyI+Pr5d733gfAVeWJvZrtcgImnVmW14eMVBNFpsoqMoDnfGFKi60YKxC3Zw4zEihZjUvyPemNhLdAxF4ZGMQM99cYQFQ6Qgq9Py8PkBjp5pC5aMIKvTcvFNRsG1n0hEbuWlrzJxjpsGthpLRoDsklq8vO6o6BhE5IQ6sw3/uyodVptddBRFYMnIzGqz47FV6ag38wIikVIdzq3EAm4N0CosGZm9s+U0Nx8jUoFFW09jH7fhuCaWjIwy86vwzubTomMQkQvYHcDjq9JR22QVHcWtsWRk0mS14YnV6Rx8SaQi+ZUN+OeGk9d+ooaxZGQyf+MpnCriHhVEavPJnvPIzOcp8CthycjgSF4V3t+RLToGEUnAZnfg+bVHYOdZistiyUjM4XDgpXWZ4N8/IvU6nFeF5XvPi47hllgyEvv8YD4O5VSKjkFEEvu/9SdRXN0oOobbYclIqKbRgtfXnxAdg4hkUNNkxSvcNv0SLBkJvfXfLJTUcJ9wIq34JqMA27kv1EVYMhI5XVyLpbvOiY5BRDJ78atMbgnwKywZicz++igsNl7tJ9Ka82X1WLiFN13/jCUjgQ1HC7Ejq1R0DCISZPG2bE5q/glLxsUaLTbM/ZYX/4i0zGyzY8EPp0THcAssGRdbvC0bueXciIxI69YdvoCsohrRMYRjybhQSU0T3t12RnQMInIDdgfwLx7NsGRc6f0d2WjgqhIi+sn3mYU4dqFadAyhWDIuUlFnxvI9HCtBRL9wOID5m7Q9pZkl4yIf/HiWu10S0SV+OF6M9NxK0TGEYcm4QFWDBct44yURXcG8jdo9mmHJuMDSnedQw93xiOgKdmSVYv85bW7VzJJpp9omKz7adVZ0DCJyc1o9mmHJtNMnu8+jst4iOgYRubk92eXYeVp7k0BYMu3QYLZhCXe8JKJWem+79j4vWDLtsHJfDsrqzKJjEJFCbM8q0dxMM5aMk8xWO97bzrv7iaj1HA5o7n46loyT1h8tRFE1NyQjorZZcyBPU/vNsGSctGp/jugIRKRAVQ0WfJWeLzqGbFgyTsgpq8euM2WiYxCRQn28WzunzFgyTliVlgMHN70kIicdvVCNA+crRMeQBUumjWx2B9ak5YmOQUQK98nuc6IjyIIl00abTxSjuIYX/Imofb7LLERZrfo/S1gybfSffbzgT0TtZ7ba8Z/9uaJjSI4l0waFVY3YeqpEdAwiUomVe3Ngt6v7Ai9Lpg3WpOXCpvK/EEQkn/zKBuxT+XRmlkwrORwOrD6g/kNbIpLX14cviI4gKZZMK+06U4bc8gbRMYhIZdZnFsJqs4uOIRmWTCt9k1EgOgIRqVBZnVnVN3ezZFrBbndg07Ei0TGISKXUfMqMJdMKB3IqUKqB9exEJMbGY0WqPWXGkmmFDZmFoiMQkYpVNViw96w6V5mxZFphwzGWDBFJa+NRdX7OsGSu4XhBNVeVEZHk1HrdlyVzDZtPFIuOQEQacKGqEUfyqkTHcDmWzDVsPcmSISJ5bFThqXmWzFVUNVhwMKdSdAwi0oidp0tFR3A5lsxVbD9VwlllRCSbI/lVaLTYRMdwKZbMVWw9yYnLRCQfi82BgyrbMZMlcxV7stU76oGI3JPa7pdhyVxBcU0j8iu5dJmI5LWPJaMNh3jBn4gEOJRbAYuKRsywZK6AJUNEIjRa7MjIqxQdw2VYMldwKEddF9+ISDnUdF2GJXMZNrsDR/LVd+ctESmDmq7LsGQu43hBNerN6lqrTkTKceB8BewquUePJXMZh3IrRUcgIg2rabTiWEG16BguwZK5DF6PISLR0lXyzS5L5jLSubKMiAQ7XVwrOoJLsGR+o7LejLNldaJjEJHGnSlhyajS0QvVcKjjehsRKdgZHsmo0zkexRCRG7hQ1Yi6JqvoGO3GkvmNnLJ60RGIiACo45QZS+Y3zrNkiMhNqOHiP0vmN3i6jIjcBUtGhXLLeSRDRO6BJaMyJTVNqOM4GSJyE7wmozI55TxVRkTu43xZveL3lmHJ/Aov+hORO7HaHYr/XGLJ/IrS/zCJSH2KqxtFR2gXlsyvnOfKMiJyM+X1ZtER2oUl8ys5XFlGRG6mok7ZJePZ2icGBwdDp9O16rnl5crc1a1M4X+YRKQ+5XUW0RHapdUls2DBgpZ/Lysrw9y5czF69GgMGjQIALB7925s2LABL774ostDyqWmUflzgohIXcrrmkRHaBedw9H2mcMTJkzAyJEjMXPmzIu+/s477+CHH37Al19+6ap8sur6/Hew2DiCmYjcx+97RePtu/uIjuE0p67JbNiwAWPGjLnk66NHj8YPP/zQ7lAiNFpsLBgicjtKvybjVMmEhIRg7dq1l3z9yy+/REhISLtDiVDdqOzznkSkTuUKL5lWX5P5tdmzZ+O+++7D1q1bW67J7NmzB+vXr8eSJUtcGlAuvB5DRO5IkyUzffp0JCcn46233sIXX3wBh8OBlJQU7Ny5EwMHDnR1RlmwZIjIHVUo/D4Zp0oGAAYOHIgVK1a4MotQNTxdRkRuqMlqR12TFX7eTn9cC+X0zZhnzpzBCy+8gMmTJ6O4uBgAsH79ehw9etRl4eRUyyMZInJTVQ3K/SbYqZLZtm0bevTogb179+Lzzz9HbW3zOOqMjAz87W9/c2lAufB0GRG5K5tduStfnSqZZ555BnPnzsWmTZvg5eXV8vWRI0di9+7dLgsnJ64uIyJ3ZdVayRw5cgR33nnnJV8PCwtDWVlZu0OJUNfEzcqIyD1p7kgmKCgIBQUFl3z90KFDiImJaXcoIiL6hb3tg1nchlMlM3nyZMyaNQuFhYXQ6XSw2+3YuXMnnnzySUydOtXVGWWhb93sTyIi2VkVPI3EqTVxr776KqZPn46YmJiWe2RsNhsmT56MF154wdUZZaFny5ALzU88hLGN34uOQSqh138IwCQ6hlOcKhmDwYAVK1Zgzpw5OHjwIOx2O/r06YOuXbu6Op9s9K3cxoCoNdZXdcb42kzRMUgtdMpd/erU6bJXXnkF9fX1SEhIwMSJEzFp0iR07doVDQ0NeOWVV1ydURY8kCFX2ljaAQ0h3UXHILXQK/NGTMDJkpk9e3bLvTG/Vl9fj9mzZ7c7lAgebBlysR99bhYdgdRC5yE6gdOcKhmHw3HZXTIPHz6MDh06tDuUCN4G5f4hknuaV9gTDgV/OJAb0Ts9nEW4Nh2D/bwFs06nQ1JS0kVFY7PZUFtbiwceeMDlIeVg9FTuHyK5pxO1vqiIH4wOBTtERyGlU/DpsjYlX7BgARwOB+69917Mnj0bgYGBLT/n5eWFuLi4ltH/SsMjGZLCtxiK/wFLhtrJy190Aqe1qWSmTZsGAIiPj8fgwYNhMBgkCSUCj2RICgvyr8Ofvf2gs9SJjkJKpfMAjEGiUzjNqU/W4cOHtxRMQ0MDqqurL3ookZFHMiSBMrMBORFcAEDt4BOk6GsyTiWvr6/HzJkzER4eDn9/fwQHB1/0UCJ/o3LPeZJ7+0+TMk8hk5vwVeaW9j9zqmSeeuopbN68GYsWLYK3tzeWLFmC2bNnIzo6Gh9//LGrM8oiPMBbdARSqffyOsHmFyk6BimVjzJX7P7MqZL5+uuvsWjRIkycOBGenp4YOnQoXnjhBbz22muK3S0zPMAI3vRPUrA59MgMGSU6BimVFo9kysvLER8fDwAwmUwoLy8HAAwZMgTbt293XToZeXnqEezrde0nEjnh3YoBoiOQUvlq8EgmISEB586dAwCkpKRg9erVAJqPcIKCglyVTXY8ZUZS+b4kFI0dkkXHICXSYsncc889OHz4MADg2Wefbbk28/jjj+Opp55yaUA5hZuMoiOQiu30u0V0BFIihZ8ua/OSKovFgnXr1mHx4sUAmrdcPnHiBNLS0pCYmIhevXq5PKRcIngkQxKaX9gTN+n00DnsoqOQkmitZAwGAzIzMy8aKRMbG4vY2FiXBhMhgkcyJKGjNX6ojBuE4MKdoqOQkmhxddnUqVPxwQcfuDqLcBEmHsmQtL7TDxcdgZTGFCU6Qbs4dQei2WzGkiVLsGnTJvTv3x9+fn4X/fz8+fNdEk5uvCZDUpufdx0mc8wMtUVIF9EJ2sWpksnMzETfvn0BAKdOnbro5y63BYBS8HQZSa3MbEBe7Eh0yvtGdBRSgoAowDtAdIp2capktmzZ4uocboGny0gO/2kajKfAkqFWCFXulvY/U+7UNQlEmozw8+KgTJLWe/mdYfMLFx2DlCA0SXSCdmPJ/IpOp8N1kco+NCX3Z7HrcCzkVtExSAlCeCSjOt2iTKIjkAYsrrhedARSAp4uU59klgzJ4JuSUDR1uE50DHJ3LBn1SebpMpIJx8zQVRl8gcBOolO0G0vmN7pFmTjyn2Txr8LecOj4vyBdQYdEqOHDiH/Df8Pf2xMdg31ExyANOFLjh6qIG0THIHelglNlAEvmspIjeV2G5PE9x8zQlYSniE7gEiyZy+AKM5LL/LxucBh8Rccgd9RJHSsQWTKXkRLFi/8kjxKzAXkRI0XHIHej9wQ6qmM3VZbMZXTj6TKS0eqmwaIjkLuJ7Al4qeMIlyVzGZ1DfBHoYxAdgzTi3fw42H3DRMcgdxI7SHQCl2HJXIZOp8MNCcreKIiUg2Nm6BKx6ll1yJK5giFdQkVHIA15v1od59/JRVgy6jeYJUMy+qooHE3Byp+4Sy7QIQHwV8+UbpbMFSSG+SMqkJuYkXx2+3PMDEFV12MAlsxVDU7k0QzJZ0FRbzig/DEi1E4qOlUGsGSuakjXENERSEPSq/1RHTFQdAwSjUcy2nEjj2RIZus9RoiOQCIFRKlmZtnPWDJXEW4yomu4v+gYpCHz8pPh8OSAVs267jbRCVyOJXMNN3KVGcmouMmA/EiOmdGs5N+JTuByLJlrYMmQ3NaYOWZGk4yBQNxQ0SlcjiVzDTckdIDBgyt+SD7v5sXB7stvbjSn62jAQ33jrFgy1xBgNPBohmTVZNfjRMgo0TFIbio8VQawZFplXI8o0RFIY5ZUcymzpngagS7qvBmXJdMKt3aPhJcH/1ORfL4oCoc5qIvoGCSXxJsALz/RKSTBT85WCPQxYGhXnjIjee0J4Ckzzeg2TnQCybBkWmlcT54yI3ktKOaYGU3QeQBJY0WnkAxLppVu7R4Jo4H/uUg+B6sCUBOhjn3e6So6Dwb81DvCip+areTv7YlRKZGiY5DGbOSYGfXrcZfoBJJiybTB+L4xoiOQxsy7kAyHJ7ecUC1vE9BjougUkmLJtMGwrmEI9fcWHYM0pKDRCwURI0THIKn0uEu1q8p+xpJpAw+9Drf3jhYdgzRmjWWI6Agklf73iE4gOZZMG03o21F0BNKYRXlxsPtwCb3qxPQDInuITiE5lkwbpUSbcH1cB9ExSEOa7HqcCOU9M6rTT/1HMQBLxin3DokXHYE05sPqAaIjkCt5m4DUCaJTyIIl44RbUyLQOcRXdAzSkM+KImEOShAdg1yl5yTASxufISwZJ+j1OkwfHCc6BmnMPo6ZUQ+NnCoDWDJOm9S/EwKMnqJjkIYsKO7LMTNq0HEAEJkqOoVsWDJO8vP2xN3Xx4qOQRqSVhWAmvD+omNQew2YITqBrFgy7TBtcBw89PzOkuTzg2GE6AjUHsFxQKq67/D/LZZMO8QE+WBMKueZkXzm5afA4cGpE4o15AnAQ1un2Vky7TSDy5lJRvmN3iiMHCE6BjkjMBboPVl0CtmxZNqpT2ww+sYGiY5BGvIZx8wo09DHAQ+D6BSyY8m4wAPDE0VHIA1ZlB8Hu4969x9RJVNHoPefRacQgiXjArd2j+TRDMmmweaBU6G3iI5BbTHkMcDTS3QKIVgyLvLsbcmiI5CGfFAzUHQEaq2AaKDvVNEphGHJuMiAuA64JTlcdAzSiDWFkbAEcsyMItz4v4CndlcEsmRcaNaYbrxvhmSz38RTZm7PPwLoN110CqFYMi7UNSIAE7hFM8nkzZK+oiPQtdz4GGDQ9vbZLBkXe2LUdTAa+J+VpLe30sQxM+4sNAm4/i+iUwjHT0MXiww0Yvpg3qBJ8viv1wjREehKxr6uyftifoslI4EHRyQiyJd/uUh6/8zrzjEz7qjb74DEm0SncAssGQkE+hjw8IguomOQBuQ1eqMocpjoGPRrnj7AmL+LTuE2WDISmTq4Mzp18BEdgzTgCyvHzLiVIY8BQdwG5GcsGYl4e3rg1Tt6iI5BGrAwLxF2Y7DoGAQAQZ2bV5RRC5aMhIYlhWFC346iY5DK1dn0yArj1sxuYczfNb9k+bdYMhJ76XcpCPXnhVmS1ke1HDMjXJdbgG7jRKdwOywZiQX6GjD7D91FxyCV+09BFCyBXDovjIcXMOZ10SncEktGBuN6RmF09wjRMUjl0kw8ZSbM4EeBUK4ovRyWjEzm3J4Kk1Fb266SvN4q7SM6gjZF9ACGzxKdwm2xZGQSbjLi+XHcDoCks7siELVhnGcmKw8v4M53NbtXTGuwZGT0xwGxGJzIHQ1JOpu9R4qOoC3DZwGRqaJTuDWWjMz+Mb4nfAweomOQSv0zvzscHvyuWhYdBwBDHhedwu2xZGQWG+KLWWOuEx2DVCqnwYjiCI6ZkZyXP3DnYkDPbxivhSUjwPQb4zEqhavNSBprbRwzI7mxbwAhiaJTKAJLRpB/TuyFmCDONiPXezuvC+zGINEx1Ct1AtBniuxvu3TpUgQFBcn+vu3FkhEk0NeAtyf3gcGD2zWTa9XZ9DjDMTPSCIoFfvevdr3E9OnTodPpLnmcPn3aRSHdC0tGoL6xwXjyVl6fIddbyjEzrqc3AOOXAMbAdr/UmDFjUFBQcNEjPl6dExtYMoL9dVgCr8+Qy60oiIbF1Fl0DHUZ+zoQ65ry9vb2RmRk5EWPN998Ez169ICfnx86deqEhx56CLW1tVd8jcOHD2PkyJEICAiAyWRCv379kJaW1vLzu3btwrBhw+Dj44NOnTrh0UcfRV1dnUvytwVLRjCdTof5k3ohIdRPdBRSmYOBt4qOoB797wMG3CfpW+j1erz11lvIzMzEsmXLsHnzZjz99NNXfP6UKVPQsWNH7N+/HwcOHMAzzzwDg6F5R94jR45g9OjRGD9+PDIyMrBq1Sr8+OOPmDlzpqS/h8vRORwOh+zvSpc4VVSDOxbuRL3ZJjoKqcSNwVVY0fCg6BjKFzcU+J8vAQ/XjIWaPn06li9fDqPxly0Bxo4dizVr1lz0vDVr1uDBBx9EaWkpgOYL/4899hgqKysBACaTCW+//TamTZt2yXtMnToVPj4+WLx4ccvXfvzxRwwfPhx1dXUXvbfUOEzLTSRFBOCNiT0xc+Uh0VFIJXZWBKKuU2/4laSLjqJcwXHApI9dVjA/GzlyJP7973+3/NjPzw9btmzBa6+9hmPHjqG6uhpWqxWNjY2oq6uDn9+lZzqeeOIJzJgxA5988gluueUW3HXXXUhMbF5WfeDAAZw+fRorVqxoeb7D4YDdbsfZs2eRnCzfiCueLnMjv+sZjb8OSxAdg1Rki/dNoiMol1cAcPd/AN8OLn9pPz8/dOnSpeVhNptx2223ITU1FZ9//jkOHDiAhQsXAgAsFstlX+Pll1/G0aNHMW7cOGzevBkpKSlYu3YtAMBut+P+++9Henp6y+Pw4cPIyspqKSK58EjGzTwzphvyKxrw7ZEC0VFIBf6Zn4pxegN09st/UNEV6PTA+PeAcHm+409LS4PVasW8efOg1zd/77969epr/rqkpCQkJSXh8ccfx913342PPvoId955J/r27YujR4+iSxfx2w/wSMbN6PU6zP9jLwxK4CBNar9zDUaURHLMTJuNfB7odptsb5eYmAir1Yq3334b2dnZ+OSTT/Duu+9e8fkNDQ2YOXMmtm7divPnz2Pnzp3Yv39/y2mwWbNmYffu3Xj44YeRnp6OrKwsrFu3Do888ohcv6UWLBk35O3pgfem9kNylEl0FFKBr+wcM9MmqROAYU/K+pa9e/fG/Pnz8frrryM1NRUrVqzA3//+9ys+38PDA2VlZZg6dSqSkpIwadIkjB07FrNnzwYA9OzZE9u2bUNWVhaGDh2KPn364MUXX0RUVJRcv6UWXF3mxoqrGzH+37uQV9EgOgopmJ+nDZl+M6FrqhIdxf11vB6Ytg4wcOSTq/BIxo2Fm4z4+N7r0cGPo9vJeXVWD5wJv0V0DPcX1Qv482csGBdjybi5hDB/fDh9AHy9OFKcnPdx3SDREdxbeErzvTAuGBlDF2PJKEDvTkFYOKUvPPUcpknO+aQgClZTJ9Ex3FNIF2DqV5IsVSaWjGKMvC4c/5jQU3QMUiiHQ8cxM5cTFAtMXQf4h4tOolosGQWZ2K8jXhgn3526pC7vlPUTHcG9BEQD074GAmNEJ1E1lozCzBiagFfvTAXPnFFbbS8PQl1Yb9Ex3INfWPMqsuA40UlUjyWjQFMGdsb8Sb15jYbabKv3SNERxPMJbr4GE9pVdBJNYMko1B19YrBwSl94efCPkFpv3oUecOg1PE3KJxj48xdARHfRSTSDn1AKNrp7JJZM6w8fA5c3U+tk1xtRGjlUdAwxAmOBezcCMX1FJ9EUlozCDUsKw8f3XY8Abw1/d0ptss6hwZKJSAXu2wiEJYlOojkcK6MSGXmVmPbhPlTUc9ouXV2ApxUZfjOha6oWHUUecUOBP63gjZaC8EhGJXp2DMKq+wchLMBbdBRyczVWT5zVypiZ7uObr8GwYIRhyahIUkQAPntgEBJCL91Fj+jXPq67QXQE6d3wEDDxQ8CTs/9E4ukyFaputOB/Pz2ELSdLREchN6XTOZAVOgueNXmio0hAB4x6BbjxUdFBCDySUSWT0YAPpg3AQyPk3WaVlMPh0CE9WIVjZvSG5h0tWTBugyWjUnq9Dk+P6YaFk/tygjNd1julKhszExDVPCam5yTRSehXeLpMA45dqMZfP0nj5md0iWMd/wHf0gzRMdovfhgw4QMOunRDPJLRgJRoE9bNHIJBCSGio5Cb2Wa8SXSEdtIBQ59s3guGBeOWeCSjIVabHa9+dxwf7TwnOgq5iS6+DdiE+6GzW0VHaTufYODO94AkFV5bUhEeyWiIp4cef/t9d/zfxJ7w9uQfPQGn631QFnGj6BhtF90XuH87C0YB+EmjQXf174RvHhmClCiT6CjkBr7GMNER2mbADODeDc0bjpHb4+kyDTNb7Zi38STe35ENO/8WaFagwYp034eha6oRHeXqvPyB378J9JgoOgm1AY9kNMzLU49nb0vG8hkDER1oFB2HBKmyeOJc+M2iY1xdwgjgwZ0sGAViyRAGJ4bi+8eGYULfjqKjkCDL6weJjnB53oHAH95u3mSMu1gqEk+X0UX+e7wIz609gqLqJtFRSEbNY2aehmdNvugov7juNmDcfMAUJToJtQOPZOgiNydHYOPjwzG+b4zoKCQjh0OHjOBRomM08w1pvrHy7k9ZMCrAIxm6oi0nijH766M4V1YvOgrJ4OaQcnxQN1NsiNSJwNg3AD/eOKwWLBm6KrPVjo92nsU7m0+jpkmBN+xRmxzv+Bp8SjPlf+OAKOB3/wKuGyv/e5OkWDLUKiU1TZi38SRWp+VyubOKLe6yF6Pz3pTvDT19gEEPATc+Bhh535YasWSoTTLzq/DKN8ew72y56CgkgSS/Bmyw/xU6h03aN9Lpgd6TgZHPA6Zoad+LhGLJkFO+zSjAa98dR34lJzurzYH4xQgp2CbdG3QZ1bypWESKdO9BboMlQ05rtNiwZEc2Fm09g3qzxN/5kmxejj+O6QVzXP/CUb2AUXOAhOGuf21yWywZarei6kYs3paNT/floMHCslG6QIMV6T4PQWeuddELxgI3vwj0uAvQ6VzzmqQYLBlymfI6M5buPItlu8+jqsEiOg61w9YuqxCX91X7XiQgGhj0MHD9XwBPb9cEI8VhyZDL1TZZsXLveSzZcRbFNZwcoER/7ZiD50qfce4XR/QABs8EUicAHgbXBiPFYcmQZJqsNnx2IA+Lt2Ujp5w3dCqJh86OUyFPwaO2oPW/KPEmYPAjzf8k+glLhiRnszvwTcYF/HvrGZwodPNx8tRibdIG9MlZdvUneXg136U/eCYQ0V2eYKQoLBmSjcPhwPasUqzen4tNx4pgttlFR6KruDW0HO/VXmHMjDEQ6HcPMPABzhejq2LJkBCV9WZ8eSgfaw7k4eiFatFx6AqOx7wKn7Kjv3yh0w3NN1GmTgC8/cUFI8VgyZBwxy5UY3VaLr5Kz0dFPVeluZP3u+zBqOrPgV5/AnpPAUISRUcihWHJkNswW+344XgR1qTlYntWKWwckiZMgNETY7pHYlKfcAxIiAD03BWEnMOSIbdUVN2IdekXsOlYEQ7kVLBwZGA06HFzcgT+0CsaI64Lg7enh+hIpAIsGXJ75XVmbD5RjE3HCrEjq5QjbFwoPtQPQ7uGYljXMAzuEgJfL0/RkUhlWDKkKI0WG/adLcf2UyXYnlWCU0UuGn2iEQFGTwxODMGwpDAM6xqGTh18RUcilWPJkKIVVDVgx6lS7DhdikM5Fcir4FToX9PrgJ4dgzCsayiGJYWhd6cgeHrw+grJhyVDqlJeZ8bhvEoczq1ERl4VMvIqUVprFh1LNh2DfZAaHYju0SZ0jzGhb2wwgny9RMciDWPJkOrlVdQjI6+qpXwy86tRq/CtpD30OiSE+qF7tAmpMYFIiTahe1QgAn05K4zcC0uGNMdud6CguhF55fXIrWhAbnk98ioakFtRj/yKBhRUNbjFFtNGgx5RgT6INBkRFWREVKARMUG+6BYVgJQoE4wGrv4i98eSIfoNi82OgspG5FbUI++n4qlutKLebEWd2Yb6pp/+abairsmGuiYr6n/68eXKSacDDHo9vD318PHygJ+3J3y9PH56eCLEzwtRQUZEBvogOtCIyEAjogN9EOzH01ykfCwZIhdqMNtgttrh6aGDp4cOBr0eej036iLtYskQEZFkuJaRiIgkw5IhIiLJsGSIiEgyLBkiIpIMS4aIiCTDkiEiIsmwZIiISDIsGSIikgxLhoiIJMOSISIiybBkiIhIMiwZIiKSDEuGiIgkw5IhIiLJsGSIiEgyLBkiIpIMS4aIiCTDkiEiIsmwZIiISDIsGSIikgxLhoiIJMOSISIiybBkiIhIMiwZIiKSDEuGiIgkw5IhIiLJsGSIiEgyLBkiIpIMS4aIiCTDkiEiIsmwZIiISDIsGSIikgxLhoiIJMOSISIiybBkiIhIMiwZIiKSDEuGiIgkw5IhIiLJsGSIiEgyLBkiIpIMS4aIiCTDkiEiIsmwZIiISDIsGSIikgxLhoiIJPP/AWpmgM/DX5qkAAAAAElFTkSuQmCC",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "df.rated.value_counts().plot.pie()\n",
        "plt.show()\n",
        "plt.close()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "80290064-537c-4d5b-a858-a23ec883fcab",
      "metadata": {
        
      },
      "source": [
        "We can affirm that most of the games are rated"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "9460194c-7598-46be-be02-4262c3a1158b",
      "metadata": {
        
      },
      "source": [
        "## What is the mean of turns that take to a white or black piece to win a game?"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "97b2d457-abb5-42f3-b090-c85322dd7772",
      "metadata": {
        
      },
      "source": [
        "### White"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 220,
      "id": "a982dd03-3886-4b0b-af01-b2d5ae6e8406",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.boxplot(data = dfwhite, x='winner', y='turns')\n",
        "plt.xlabel('Winner')\n",
        "plt.ylabel('Turns')\n",
        "plt.show()\n",
        "plt.close()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 226,
      "id": "a1908278-a4ab-45a7-82e2-f8f4e958ddc6",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "58"
            ]
          },
          "execution_count": 226,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "round(dfwhite.turns.mean())"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "87682ea8-409c-4dcc-beef-0a180e24d113",
      "metadata": {
        
      },
      "source": [
        "We can see that the mean turns to win a game as a white piece is 58"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "93b2e397-dfcd-4041-bbb0-4507f2901675",
      "metadata": {
        
      },
      "source": [
        "We can also see that there are a lot of values that go out of the chart. So, for better visualization, we are going to use the interquantile range"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 217,
      "id": "f97136ff-59c3-435e-a8c5-c4e6e31098f2",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "Q1 = dfwhite['turns'].quantile(0.25)\n",
        "Q3 = dfwhite['turns'].quantile(0.75)\n",
        "IQR = Q3 - Q1"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 222,
      "id": "987cab0a-b55d-4d48-a39e-7437a51bf7e7",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfwhiteinterquantile = dfwhite[dfwhite['turns'].between(Q1 - 1.5 * IQR, Q3 + 1.5 * IQR)]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 223,
      "id": "30d85a04-42f2-41d3-bf98-ba7c0f9e828f",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.boxplot(data = dfwhiteinterquantile, x='winner', y='turns')\n",
        "plt.xlabel('Winner')\n",
        "plt.ylabel('Turns')\n",
        "plt.show()\n",
        "plt.close()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "eda4a434-40f5-4dc7-a21b-ce85d838a226",
      "metadata": {
        
      },
      "source": [
        "### Black"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 227,
      "id": "0fdc7f8c-f58f-4c71-a2c4-2628c0fe454f",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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v/1N48aN04QJE/TKK68oLKzlKYwjRozQu+++q40bN2revHmdtn8AOhdXYwEAAKMxswMAAIxG2AEAAEYj7AAAAKMRdgAAgNEIOwAAwGiEHQAAYDTCDgAAMBphBwAAGI2wA6Bb2rFjhyzL0tGjR51uBUCAI+wAcNwTTzyh6OhoNTY2erdVV1erR48euvjii33G/uUvf5FlWYqPj1dJSYliYmK6ul0A3QxhB4DjJk6cqOrqau3Zs8e77S9/+Yvi4uJUUFCg//3vf97tO3bsUHx8vJKSkhQXFyfLspxo2aupqUkej8fRHgCcGGEHgONGjBih+Ph47dixw7ttx44duuqqqzRs2DDl5eX5bJ84cWKLw1hr165V79699dZbb+mss87Saaedpssvv1wlJSXe19566626+uqr9cc//lGDBg1Sv379dNddd6mhocE7pr6+XgsXLtTpp5+uqKgopaSk+PR1/HO2bNmis88+Wy6XS59//nmn/dkA6DjCDoCAkJ6ertzcXO/z3NxcpaenKy0tzbu9vr5eH3zwgSZOnNjqe/zvf//TH//4R61fv17vvfeeCgsLNX/+fJ8xubm5+ve//63c3FytW7dOa9eu1dq1a731n/70p3r//fe1adMmffzxx/rJT36iyy+/XJ988onP52RlZempp57SwYMHNXDgQD/+SQDwN8IOgICQnp6u999/X42NjaqqqtJHH32kH/zgB0pLS/POrOzevVs1NTVthp2GhgY98cQTGjt2rM4//3zNnj1b77zzjs+YPn36KDs7W9///vc1depUXXHFFd4x//73v7Vx40a98MILuvjiizVs2DDNnz9fF110kZ555hmfz3n88ceVmpqqESNGKCoqqnP+UAD4RZjTDQCA9M15O8eOHVNBQYEqKiqUlJSkgQMHKi0tTTfddJOOHTumHTt2KDExUd/73vdUWFjY4j0iIyM1bNgw7/NBgwaprKzMZ8zIkSMVGhrqM2b//v2SpA8//FC2bSspKcnnNXV1derXr5/3ec+ePXXOOef4Zb8BdD7CDoCAcOaZZ2rw4MHKzc1VRUWF0tLSJElxcXEaOnSo3n//feXm5uqSSy5p8z169Ojh89yyLNm2fdIxx08w9ng8Cg0N1d69e30CkSSddtpp3p8jIiIcPzEawKkj7AAIGMdPPK6oqNCCBQu829PS0vTWW29p9+7d+ulPf9ppnz969Gg1NTWprKysxSXvALovztkBEDAmTpyoXbt2ad++fd6ZHembsPPkk0+qtra2zfN1/CEpKUk33nijbr75Zr300ks6fPiwCgoK9NBDD+mNN97otM8F0LkIOwACxsSJE1VTU6MzzzxTsbGx3u1paWmqqqrSsGHDlJCQ0Kk9PPPMM7r55ps1b948jRgxQtOmTVN+fn6nfy6AzmPZ3z6gDQAAYBBmdgAAgNEIOwAAwGiEHQAAYDTCDgAAMBphBwAAGI2wAwAAjEbYAQAARiPsAAAAoxF2AACA0Qg7AADAaIQdAABgtP8HZe8yop2+J4oAAAAASUVORK5CYII=",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.boxplot(data = dfblack, x='winner', y='turns')\n",
        "plt.xlabel('Winner')\n",
        "plt.ylabel('Turns')\n",
        "plt.show()\n",
        "plt.close()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 228,
      "id": "19227841-c6ad-44c4-ac4c-e6cb7b3dd52f",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "61"
            ]
          },
          "execution_count": 228,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "round(dfblack.turns.mean())"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "92203285-4490-42cd-9519-7176c1752ad3",
      "metadata": {
        
      },
      "source": [
        "As we can see, black pieces, as a mean, take 61 turns to win a game. Now, to better visualize this one as well, we are going to use the interquantile range."
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 229,
      "id": "f6467808-e106-4088-bbbc-80e101dc989a",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "Q1_1 = dfblack['turns'].quantile(0.25)\n",
        "Q3_1 = dfblack['turns'].quantile(0.75)\n",
        "IQR = Q3_1 - Q1_1"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 230,
      "id": "fc2c60d8-a139-4eb6-ba2f-612a0d9c2112",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfblackinterquantile = dfblack[dfblack['turns'].between(Q1_1 - 1.5 * IQR, Q3_1 + 1.5 * IQR)]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 231,
      "id": "8854b42a-5a0f-4a9c-b618-5aba7caa3799",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.boxplot(data = dfblackinterquantile, x='winner', y='turns')\n",
        "plt.xlabel('Winner')\n",
        "plt.ylabel('Turns')\n",
        "plt.show()\n",
        "plt.close()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "e2d420b2-9f5e-438d-84c4-c73c1aeb2d2a",
      "metadata": {
        "tags": [
          
        ]
      },
      "source": [
        "## Rating difference related to wins"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 242,
      "id": "d9dd46df-24b6-493f-b71b-289b485071fc",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfwhiterating = dfwhite\n",
        "dfblackrating = dfblack"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 250,
      "id": "940a7215-0140-48e8-a1a1-22d6f3737e6a",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "-88.98111342923026"
            ]
          },
          "execution_count": 250,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfblackrating.rating_change.mean()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 251,
      "id": "fa832d7b-0e1b-4dff-8af0-2233c39a9a9d",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "95.30746925307469"
            ]
          },
          "execution_count": 251,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfwhiterating.rating_change.mean()"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "9bc7e1ee-af85-43ee-a9aa-333832632772",
      "metadata": {
        
      },
      "source": [
        "With this we can see that, when white pieces win, the rating of the players, as a mean, is 95.30 positive to white. In the other hand, when black pieces win, the rating is 88.98 positive to black rating pieces"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "c7eb20f9-ed7f-4c29-b649-9c3b765aa77d",
      "metadata": {
        
      },
      "source": [
        "## Which is the most winning black id and white id"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "4a618160-a19c-45aa-a736-89354aa30a7a",
      "metadata": {
        
      },
      "source": [
        "### White"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 264,
      "id": "e7bcfa1e-16f3-48ab-ad95-c80027447714",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "taranga               34\n",
              "ssf7                  29\n",
              "hassan1365416         28\n",
              "a_p_t_e_m_u_u         25\n",
              "vladimir-kramnik-1    22\n",
              "1240100948            22\n",
              "lance5500             21\n",
              "traced                21\n",
              "ozil17                21\n",
              "ozguragarr            21\n",
              "Name: white_id, dtype: int64"
            ]
          },
          "execution_count": 264,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfwhite.white_id.value_counts().head(10)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 269,
      "id": "3ca39317-07ac-4152-89bb-c2836aa44a48",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfwinningwhiteid = dfwhite[dfwhite.white_id.isin(['taranga', \\\n",
        "                         'ssf7', \\\n",
        "                         'hassan1365416', \\\n",
        "                         'a_p_t_e_m_u_u', \\\n",
        "                         'vladimir-kramnik-1', \\\n",
        "                         '1240100948', \\\n",
        "                         'lance5500', \\\n",
        "                         'traced', \\\n",
        "                         'ozil17', \\\n",
        "                         'ozguragarr'])]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 271,
      "id": "1a8a3ea0-a1c8-4d1c-87a0-7d00c93797ac",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "array(['taranga', 'lance5500', 'a_p_t_e_m_u_u', 'traced', 'hassan1365416',\n",
              "       'ozguragarr', '1240100948', 'vladimir-kramnik-1', 'ozil17', 'ssf7'],\n",
              "      dtype=object)"
            ]
          },
          "execution_count": 271,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfwinningwhiteid.white_id.unique()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 272,
      "id": "e6bcba98-c4ec-45c3-87a0-c4112e868c37",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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P/+s5cuXa9u2bRo9erRl3VdffVVJSUnasWOHjh49qvfff98S0saPH69jx45pw4YNOn78uBYsWKBSpUpZ1i1WrJiWLVumY8eOafbs2Vq8eLE++ugjq9pOnz6ttWvXav369Vq/fr0iIiI0bdq0v72vd+7c0aJFi+Ti4qK6detKkmJjY7Vv3z65u7srICBApUuXVrNmzbRr164067///vsqWbKk6tWrp6lTp+rOnTtWy48dO6ZJkybp888/TzO6mlnr1q2Tn5+fpk+frrJly6pq1aoaOXKk1e0Df5WUlKT4+HirCQAA5F/57pJ9nTp19O6770qSqlSponnz5mnr1q1q06aN1QNPFStW1OTJkzVkyBDNnz9fkhQdHa3nn39etWvXliRVqlTJ0j46Olr169eXn5+fpHv3YD5o3Lhxls/e3t5688039c0331gF3tTUVC1btkzFihWTJPXp00dbt27V1KlTs7SP69evV48ePXTz5k15enoqLCzMEo7PnDkj6d6I7Icffqh69erp888/V6tWrfTzzz+rSpUqkqQ33nhDTzzxhEqUKKH9+/crODhYZ8+e1aeffirpXijs2bOnPvjgA5UvX97Sb1adOXNGu3btUpEiRbRmzRpdvnxZr7zyiq5evZrhfaQhISGaOHHi39oeAADIe/JlIH2Qp6enYmNjJUnbt2/Xe++9p2PHjik+Pl4pKSm6ffu2EhMT5ejoqNdff11DhgzR5s2b1bp1az3//POW/oYMGaLnn39ehw8fVtu2bfXss89aXXb+7rvvNGvWLJ06dUoJCQlKSUmRs7OzVS3e3t6WMPrX2rKiRYsWioqK0uXLl7V48WJ169bNMiqampoqSXr55Zf14osvSpLq16+vrVu3aunSpQoJCZF077L+g8esRIkS6tq1q2XUNDg4WDVq1NC//vWvLNf3oNTUVJlMJq1YsUIuLi6S7t060bVrV3388cdycHBIs05wcLBGjBhh+R4fHy8vL69/VAcAAMi98t0le1tbW6vvJpNJqampOn/+vIKCglSrVi2Fhobq0KFD+vjjjyXJcq/nv//9b505c0Z9+vTR0aNH5efnp7lz50q6d3/n+fPnNWzYMF28eFGtWrWy3Fe5d+9e9ejRQ+3bt9f69esVGRmpsWPHprkEnlFtWeXo6CgfHx81atRIS5YsUeHChbVkyRJJ90KuJPn6+lqt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EvOxY8fMw4YNMzs6OprPnTtndGmGePPNN83h4eHmM2fOmPfu3Wt+5plnzMWKFStwx+PGjRvmyMhIc2RkpFmSeebMmebIyEjz+fPnzWaz2Txt2jSzi4uLefXq1eajR4+ae/bsafb09DTHx8cbXHnOedgxuXHjhvnNN980796923z27Fnz9u3bzf7+/uayZcvm62MyZMgQs4uLizk8PNwcExNjmW7evGlpUxDPlUcdl4J4vgQHB5t37NhhPnv2rPnIkSPmt99+21yoUCHz5s2bzWZzwTxPzOaHH5fsPk8IpMj1Pv74Y3OFChXMdnZ25ieeeMLq1SQFTffu3c2enp5mW1tbc5kyZcxdunQx//LLL0aX9dht377dLCnN1K9fP7PZfO91Pu+++67Zw8PDbG9vb27atKn56NGjxhadwx52TG7evGlu27at2c3NzWxra2suX768uV+/fubo6Gijy85R6R0PSebPPvvM0qYgniuPOi4F8XwZMGCA5e+Mm5ubuVWrVpYwajYXzPPEbH74ccnu88RkNpvNWR9XBQAAALIH95ACAADAUARSAAAAGIpACgAAAEMRSAEAAGAoAikAAAAMRSAFAACAoQikAAAAMBSBFAAAAIYikAIAAMBQBFIAwN926dIlvfbaa6pUqZLs7e3l5eWljh07auvWrY+1DpPJpLVr1z7WbQLIPoWNLgAAkDedO3dOjRs3VvHixTV9+nTVqVNHycnJ2rRpk1599VX9+uuvRpcIII/gt+wBAH9LUFCQjhw5ohMnTsjR0dFq2fXr11W8eHFFR0frtdde09atW1WoUCG1a9dOc+fOVenSpSVJ/fv31/Xr161GN4cNG6aoqCiFh4dLkpo3b646deqoSJEi+vTTT2VnZ6fBgwdrwoQJkiRvb2+dP3/esn6FChV07ty5nNx1ANmMS/YAgCy7evWqNm7cqFdffTVNGJWk4sWLy2w269lnn9XVq1cVERGhsLAwnT59Wt27d8/y9pYvXy5HR0ft27dP06dP16RJkxQWFiZJOnDggCTps88+U0xMjOU7gLyDS/YAgCw7deqUzGazqlevnmGbLVu26MiRIzp79qy8vLwkSV988YVq1qypAwcO6Mknn8z09urUqaN3331XklSlShXNmzdPW7duVZs2beTm5ibpXgj28PD4B3sFwCiMkAIAsuz+3V4mkynDNsePH5eXl5cljEqSr6+vihcvruPHj2dpe3Xq1LH67unpqdjY2Cz1ASD3IpACALKsSpUqMplMDw2WZrM53cD64PxChQrpr48yJCcnp1nH1tbW6rvJZFJqaurfKR1ALkQgBQBkmaurqwIDA/Xxxx8rMTExzfLr16/L19dX0dHRunDhgmX+sWPHFBcXpxo1akiS3NzcFBMTY7VuVFRUluuxtbXV3bt3s7wegNyBQAoA+Fvmz5+vu3fvqmHDhgoNDdVvv/2m48ePa86cOfL391fr1q1Vp04d9e7dW4cPH9b+/fvVt29fNWvWTH5+fpKkli1b6uDBg/r888/122+/6d1339XPP/+c5Vq8vb21detWXbp0SdeuXcvuXQWQwwikAIC/pWLFijp8+LBatGihN998U7Vq1VKbNm20detWLViwwPKy+hIlSqhp06Zq3bq1KlWqpG+++cbSR2BgoMaPH6/Ro0frySef1I0bN9S3b98s1zJjxgyFhYXJy8tL9evXz87dBPAY8B5SAAAAGIoRUgAAABiKQAoAAABDEUgBAABgKAIpAAAADEUgBQAAgKEIpAAAADAUgRQAAACGIpACAADAUARSAAAAGIpACgAAAEMRSAEAAGCo/wePbCFnCMQeewAAAABJRU5ErkJggg==",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.countplot(y='white_id', \\\n",
        "              data = dfwinningwhiteid, \\\n",
        "              orient='h', \\\n",
        "              order=dfwinningwhiteid['white_id'].value_counts().index)\n",
        "plt.ylabel('White Piece ID')\n",
        "plt.xlabel('Count')\n",
        "plt.show()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 273,
      "id": "bf01dfb6-d768-4636-b96e-dd90aedde1be",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "taranga               0.0034\n",
              "ssf7                  0.0029\n",
              "hassan1365416         0.0028\n",
              "a_p_t_e_m_u_u         0.0025\n",
              "vladimir-kramnik-1    0.0022\n",
              "1240100948            0.0022\n",
              "lance5500             0.0021\n",
              "traced                0.0021\n",
              "ozil17                0.0021\n",
              "ozguragarr            0.0021\n",
              "Name: white_id, dtype: float64"
            ]
          },
          "execution_count": 273,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfwhite.white_id.value_counts(normalize=True).head(10)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "0ace511d-0667-4d0e-a7b3-31eda2644c8a",
      "metadata": {
        
      },
      "source": [
        "For white pieces, we have that the most winning id is **taranga**. This id won 34 games, representing 0.34% of the total of games"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "67014156-dcb1-4622-b68b-846a9054d89c",
      "metadata": {
        
      },
      "source": [
        "### Black"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 274,
      "id": "2963416a-e415-4a80-816a-32cdf16f9f80",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "taranga               38\n",
              "ducksandcats          29\n",
              "vladimir-kramnik-1    28\n",
              "chesscarl             27\n",
              "docboss               25\n",
              "king5891              22\n",
              "smilsydov             21\n",
              "a_p_t_e_m_u_u         21\n",
              "doraemon61            20\n",
              "saviter               18\n",
              "Name: black_id, dtype: int64"
            ]
          },
          "execution_count": 274,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfblack.black_id.value_counts().head(10)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 279,
      "id": "89f7ff0f-1490-4419-a73f-41c55be4b114",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfwinningblackid = dfblack[dfblack.black_id.isin(['taranga', \\\n",
        "                         'ducksandcats', \\\n",
        "                         'vladimir-kramnik-1', \\\n",
        "                         'chesscarl', \\\n",
        "                         'docboss', \\\n",
        "                         'king5891', \\\n",
        "                         'smilsydov', \\\n",
        "                         'a_p_t_e_m_u_u', \\\n",
        "                         'doraemon61', \\\n",
        "                         'saviter'])]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 280,
      "id": "78bf7225-fa51-40a4-b107-0e97056ad08e",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "array(['taranga', 'chesscarl', 'saviter', 'ducksandcats', 'a_p_t_e_m_u_u',\n",
              "       'doraemon61', 'king5891', 'vladimir-kramnik-1', 'docboss',\n",
              "       'smilsydov'], dtype=object)"
            ]
          },
          "execution_count": 280,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfwinningblackid.black_id.unique()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 281,
      "id": "27dfe3f4-2864-4109-b676-020166d7032c",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.countplot(y='black_id', \\\n",
        "              data = dfwinningblackid, \\\n",
        "              orient='h', \\\n",
        "              order=dfwinningblackid['black_id'].value_counts().index)\n",
        "plt.ylabel('Black Piece ID')\n",
        "plt.xlabel('Count')\n",
        "plt.show()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 282,
      "id": "247f1e21-6ae6-4b53-93e2-770347962569",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "taranga               0.004173\n",
              "ducksandcats          0.003184\n",
              "vladimir-kramnik-1    0.003075\n",
              "chesscarl             0.002965\n",
              "docboss               0.002745\n",
              "king5891              0.002416\n",
              "smilsydov             0.002306\n",
              "a_p_t_e_m_u_u         0.002306\n",
              "doraemon61            0.002196\n",
              "saviter               0.001977\n",
              "Name: black_id, dtype: float64"
            ]
          },
          "execution_count": 282,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfblack.black_id.value_counts(normalize=True).head(10)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "ee81b0db-b21e-4dc2-946a-c853627adb38",
      "metadata": {
        
      },
      "source": [
        "For black pieces, we can see that **taranga** is also the more winning player, with 38 games. This represent 0.41% of the total games"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "91ca89e4-4895-451d-bce4-ec5b45b3db39",
      "metadata": {
        
      },
      "source": [
        "## Is there a relation between rating difference and the number of turns to win a game?"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 308,
      "id": "b0e7b848-ea87-4678-bbc9-87d8fe081aab",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "Text(0.5, 1.0, 'Relation between Rating Change and Turns')"
            ]
          },
          "execution_count": 308,
          "metadata": {
            
          },
          "output_type": "execute_result"
        },
        {
          "data": {
            "image/png": "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vql1fcvLy0VJSYm4du2aACC+/PJL6VhtP2f79u0TAHR+XwghxMKFCwUAMW/evFqXp+L33oN1e/j3y8PlexAAoVKppJ/nh/OdNGmSzv4lS5YIACIzM1MI8b97mJaWVusyk2ljS0wj6969OywtLWFvb4+BAwfC0dERX375JSws7g9HunLlCn788Ue88sorAIDS0lJpe+6555CZmVnlX4YV8vPz8fbbb+PJJ5+EhYUFLCws0KJFCxQUFODixYv1KnPFgNExY8bo7H/ppZdgZ2dXqVWhc+fOaNWqlfTa2toabdq00enS0mfo0KGwtraWXtvb22Pw4MH47rvvUFZWhnv37uHbb7/FkCFDYGtrW+ke3bt3D8eOHavxOv369cN//vMfFBYW4tq1a7hy5QpGjhyJzp07IyEhAcD91plWrVrB29sbALBnzx4oFAr89a9/1bmuWq1Gp06dpJliycnJuH37NsLDw3XSlZeXY+DAgTh58mSlroKwsDCd1x07dsS9e/eQlZVVY10KCgpgaWkJS0tLuLi44I033sCIESMqtXCUlpYiJiYGTz31FKysrGBhYQErKytcvny53p+PCrV535OSktC3b1+4uLhI+8zMzDB8+PAGXVufPn36wN7eXnrt5uYGV1dXnXLV9b5U9V4BkPI8duwYioqKKtWre/fulbpS9uzZAx8fH3Tu3FnnsxIcHFyr2YdZWVmYOHEiPDw8YGFhAUtLS3h6egJArcv+4Ofs0KFDACD9DqowatQoveVoLH379q120kNN70Pnzp1hZWWF119/HZs2baqyy5eaFw7sbWSffvop2rdvj7y8POzYsQMff/wxXn75Zezbtw/A/8YvzJgxAzNmzKgyj4f7uh80atQofPvtt5g7dy66desGBwcHKBQKPPfccygsLKxXmW/dugULCws8+uijOvsVCgXUajVu3bqls9/Z2blSHkqlstbXV6vVVe4rLi5Gfn4+8vPzUVpaipUrV2LlypVV5qHvHlXo378/5s+fjyNHjuDatWtwcXFBly5d0L9/fyQmJuJvf/sbvv32W52upJs3b0IIATc3tyrzrOi6qXgfX3zxxWqvf/v2bdjZ2UmvH75vSqUSAGp132xsbKRuG61Wi6VLl+Kzzz5Dx44dMWvWLCldZGQkPvroI7z99tvo1asXHB0dYWZmhnHjxtX781Fd+Svq8GC+t27d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gSeffBIAsHv3brz44ot4/fXX8cwzz1QaoEtERET119C1XuRcJ8YY1Xlgb4sWLXDr1i0AwIEDB6Q+LmtraxQWFspbOiIiIqJq1LklZsCAARg3bhy6dOmCn376SRobc/78ebRu3Vru8hEREf2J1eMpjpXOb77q3BLz0UcfISAgAL///jt27twJZ2dnAPeXGn755ZdlLyAREdGfVhM/xdrU1Lkl5pFHHqlyBDEfqEhERERNqV7rxNy5cwcnTpxAVlYWysvLpf0KhaJej9ImIiKiqrA7SZ86BzFff/01XnnlFRQUFMDe3r7SsxAYxBAREcmFQYw+dR4TExUVhddeew15eXm4c+cOsrOzpe327duNUUYiIiKiSurcEvPrr79i2rRpsLW1bYzyEBERUYUmXrHX1NS5JSY4OBgpKSmNURYiIiJ6gJBha87q3BIzaNAgvPXWW7hw4QJ8fX1haWmpczwsLEy2whERERFVp85BzPjx4wEACxYsqHRMoVCgrKys4aUiIiIidifVoM5BzINTqomIiKgxcXaSPnUaE1NaWgoLCwucO3euscpDREREFSpaYhqyNWN1CmIsLCzg6enJLiMiIiIyuDp3J7377ruYPXs2tmzZAicnp8YoE5HJ2Xki29BF0FEqU69vabk8f8UpzeWZI2Fsf1Qa2/sOAMOedjR0EYiaTJ2DmH/84x+4cuUKNBoNPD09YWdnp3P81KlTshWOiIjoT40De/WqcxDzwgsvNEIxiIiIiOqmzkHMvHnzGqMcREREVAlnJ+lTr6dYExERURNp5l1CDVHnIMbMzEznydUP48wlIiIiagp1DmJ27dql87qkpASnT5/Gpk2bMH/+fNkKRkRERKRPnYOY559/vtK+F198ER06dMCOHTswduxYWQpGRET0p8fZSXrV+SnW1fH390diYqJc2RERERHpJcvA3sLCQqxcuRItW7aUIzsiIiICwNlJ+tU6iHnttdewYsUKeHp66gzsFUIgLy8Ptra22LJlS6MUkoiI6E+J3Ul61TqI2bRpExYtWoTly5frBDFmZmZ49NFH4e/vD0dHLndNRERETaPWQYwQ9599MmbMmMYqCxEREVGt1WlMjL71YYiIiEhm7E7Sq05BTJs2bWoMZG7fvt2gAhEREVEFDuzVp05BzPz586FSqRqrLERERES1VqcgZuTIkXB1dW2sshAREdGD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Lx3//PPPMX36dLz22ms4cOAA0tLS8Oqrr6K4uLhe13uwq6oi+Hiwu+bhgEQIoTe/Nm3a4Oeff0ZJSUmdrl1xrQevPXjwYNy6dQvr16/H8ePHcfz4cQCoVFd9dagqqHq4DuXl5Zg/fz7S0tKk7ezZs7h8+TKsra1rrAcRmRYGMURNZN68efj73/+O3377DQDw/fffIzAwEJMmTUKXLl3w5JNP6gx2BQArKyuUlZU1+Npt27bFiRMndPalpKToPWfUqFHIz8/H6tWrqzx+586dWl371q1buHjxIt59913069cP7du3r/NUcwBo165djXX4y1/+gkuXLuHJJ5+stJmZ8dcdUXPT+HM0iQgA0Lt3b3To0AExMTFYtWoVnnzySXz66afYv38/vLy8sHnzZpw8eVLqagHud9Ps378fly5dgrOzM1QqVb2uPXXqVIwfPx5du3ZFYGAgduzYgR9++AGPP/54tef4+/tj5syZiIqKwq+//oohQ4ZAo9HgypUrWLt2LXr06IE333yzxms7OjrC2dkZ69atg7u7O65fv45Zs2bVqw49e/bEsmXLMHjwYBw8eBD79u3TaZ157733EBoaCg8PD7z00kswMzPDDz/8gLNnz+L999+v8zWJyLjxTxOiJhQZGYn169cjIyMDEydOxNChQzFixAj4+/vj1q1blWYCjR8/Hm3btpXGzfznP/+p13VfeeUVzJ49GzNmzMBf/vIXpKenY8yYMTV2sSxevBjbtm3D8ePHERwcjA4dOiAyMhIdO3asNMW6OmZmZti+fTtSU1Ph4+OD6dOn44MPPqhzHZ555hmsXbsWy5YtQ6dOnRAfH4/p06fr1CE4OBh79uxBQkICunXrhu7du2PZsmXw9PSs8/WIyPgpRE0d40TULA0YMABqtdqkF60bP348fvzxR3z//feGLgoRGQC7k4j+BO7evYu1a9ciODgY5ubm+Oyzz5CYmIiEhARDF61O/v73v2PAgAGws7PDvn37sGnTpmrH7BBR88eWGKI/gcLCQgwePBinTp1CUVER2rZti3fffRdDhw41dNHqZPjw4Th8+DDy8vLw+OOPY+rUqZg4caKhi0VEBsIghoiIiEwSB/YSERGRSWIQQ0RERCaJQQwRERGZJAYxREREZJIYxBAREZFJYhBDREREJolBDBEREZkkBjFERERkkhjEEBERkUn6f44RVravRlaQAAAAAElFTkSuQmCC",
            "text/plain": [
              "<Figure size 640x480 with 2 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.histplot(data=df, x='rating_change', y='turns', bins=20, cbar=True)\n",
        "plt.xlabel('Rating Change')\n",
        "plt.ylabel('Turns')\n",
        "plt.title('Relation between Rating Change and Turns')"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "823fc943-c722-4bd8-8152-a9a19a83d58f",
      "metadata": {
        
      },
      "source": [
        "We can see in the graph that most of the games have two things in common:\n",
        "```\n",
        "Rating change is small\n",
        "Turns are lower than 50\n",
        "```\n",
        "We can also say:\n",
        "```\n",
        "High rating difference games are very short\n",
        "High turns games have small rating difference\n",
        "```"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "1644c389-f5ed-43cb-b9ce-32b95cddc4a4",
      "metadata": {
        
      },
      "source": [
        "## What is the most winning Opening Eco"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 312,
      "id": "2394eb2c-a5bd-4849-96d1-fd9b0c980fcc",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "A00    1007\n",
              "C00     844\n",
              "D00     739\n",
              "B01     716\n",
              "C41     691\n",
              "C20     675\n",
              "A40     618\n",
              "B00     611\n",
              "B20     567\n",
              "C50     538\n",
              "Name: opening_eco, dtype: int64"
            ]
          },
          "execution_count": 312,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.opening_eco.value_counts().head(10)"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 314,
      "id": "16c9e1e4-148b-4361-90ff-f0a09ab13081",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        
      ],
      "source": [
        "dfopeningeco = df[df.opening_eco.isin(['A00', 'C00', 'D00', 'B01', 'C41', 'C20', 'A40', 'B00', 'B20', 'C50'])]"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 316,
      "id": "7557bdbd-e898-4b9b-8a50-b00663b43c0c",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "array(['B00', 'C20', 'C41', 'D00', 'C50', 'B01', 'A00', 'C00', 'A40',\n",
              "       'B20'], dtype=object)"
            ]
          },
          "execution_count": 316,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "dfopeningeco.opening_eco.unique()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 317,
      "id": "000db7db-6151-4faf-93db-183ab53d706e",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "image/png": "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vn37pjodAAAQooIeeuLi4rRt2zbl5ORo1qxZqqioUNeuXZWUlKRFixad93Eee+wx/elPf7I+Dxw4UJL03nvvcRkMAADIYTbwfRI7duyo/0AOh9q0aaOePXs225uDvV6vDMPQnMzlautsF+xyACCops8bH+wSgPNy5vfb4/EoMjLyrO0aPNLzox/96JyzrVq1aqWbbrpJL7zwgtq0adPQwwMAADSKBt/IvGrVKvXt21cvvviiSkpKtH37dr344ovq16+f/vznP2vJkiVav369fvWrXzVGvQAAABekwSM9OTk5+v3vf6/U1FRrXWJionr06KFHH33Uep3ErFmzNHfu3IAWCwAAcKEaPNKzc+dO9erVq876Xr16aefOnZK+vQRWUVFx8dUBAAAESINDT//+/fXUU0/p5MmT1rqamho99dRT1uskvvzyS0VHRweuSgAAgIvU4Mtbzz//vCZMmKAePXooMTFRDodDO3bsUG1trdasWSNJ+uyzzzRt2rSAFwsAAHChGhx6hg8frrKyMr366qv65JNPZJqmbrjhBk2aNEkRERGSpIyMjIAXCgAAcDEa/JyeS9n5zvMHAACho9Ge0yNJn3zyid5//30dPnxYp0+f9tv22GOPXcghAQAAGlWDQ8/ixYt1zz33qEuXLnK5XH4PKnQ4HIQeAAAQkhocen7zm98oJydHDz74YGPUAwAA0CgaPGW9qqpKEydObIxaAAAAGk2DQ8/EiRO1du3axqgFAACg0TT48lafPn306KOPqqioSFdccYVatWrltz0rKytgxQEAAARKg6esx8XFnf1gDoc+++yziy4qWJiyDgBA89NoU9b3799/UYU1B09PzVCb741gAcCl5pFX/xrsEoAm1eB7egAAAJqj8xrpmTlzpn79618rPDxcM2fOPGfbZ555JiCFAQAABNJ5hZ7t27erpqbG+vfZfPdBhQAAAKHkvELPe++9V++/AQAAmgvu6QEAALbQ4Nlbx48f11NPPaWCgoJ6XzjanKesAwCAS1eDQ8/Pf/5zbdiwQRkZGerWrRv38QAAgGahwaHn7bff1ptvvqkRI0Y0Rj0AAACNosH39HTs2FGdOnVqjFoAAAAaTYNDz69//Ws99thj+uabbxqjHgAAgEbR4NAzb948vfPOO4qOjtYVV1yhQYMG+S0Xwu12a8aMGerdu7ecTqdiY2M1fvx4FRQUqLKyUjNmzFC/fv3Url079ezZU1lZWfJ4PH7HqKqqUkZGhgzDkGEYysjIUHV19QXVAwAALj0NvqfnuuuuC2gBZWVlGjFihDp06KA5c+YoMTFRNTU1euedd5SZmam//vWvOnjwoObOnasBAwbo888/1913362DBw/qr3/9v/fGTJo0SV988YXy8/MlSXfddZcyMjL0t7/9LaD1AgCA5qnBb1kPtGuuuUY7duzQnj17FB4e7returpaHTp0qLPPa6+9psmTJ+v48eMKCwtTaWmpBgwYoKKiIg0dOlSSVFRUpOTkZH388cfq169fvd/t8/nk8/msz16vV7GxsfrVjRN44SiASx4vHMWl4nzfsn5BDyesrq7WH//4R2VnZ6uyslKStG3bNn355ZcNOk5lZaXy8/OVmZlZJ/BIqjfwSLJOKizs24GqwsJCGYZhBR5JGjZsmAzD0KZNm876/bm5udblMMMwFBsb26D6AQBA89Hg0LNjxw79x3/8h/7nf/5Hc+fOte6bWbVqlbKzsxt0rH379sk0TfXv3/+89/n666/161//Wv/93/9trXO73YqKiqrTNioqSm63+6zHys7OlsfjsZby8vIG1Q8AAJqPBoeemTNnasqUKdq7d6/atGljrb/66qv1wQcfNOhYZ66sne8DDr1er6699loNGDBAjz/+uN+2+o5hmuY5j+10OhUZGem3AACAS1ODQ8+WLVv8RlnO6N69+zlHVerTt29fORwOlZaW/tu2R48eVVpamtq3b69Vq1ap1XfuuXG5XDp06FCdfY4cOaLo6OgG1QQAAC5NDQ49bdq0kdfrrbN+z5496tq1a4OO1alTJ6Wmpur555/X8ePH62w/c+nM6/UqJSVFrVu31urVq/1GmCQpOTlZHo9HxcXF1rrNmzfL4/Fo+PDhDaoJAABcmhocen76059q9uzZqqmpkfTtZaUDBw7ooYce0vXXX9/gAhYuXKja2loNGTJEK1eu1N69e1VaWqr58+crOTlZR48eVUpKio4fP64lS5bI6/XK7XbL7XartrZWkhQfH6+0tDRNnTpVRUVFKioq0tSpU5Wenn7WmVsAAMBeGjxl3ev16pprrtFHH32ko0ePKiYmRm63W8nJyXrrrbfqnYX171RUVCgnJ0dr1qxRRUWFunbtqqSkJN1///2SpDFjxtS73/79+3XZZZdJ+nYm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AhAsjPQACFlnrr47HI6ztiktLVVsbKwVeCRpwIAB6tChg0pLSxv0fYmJiX6fu3XrpsOHDzfoGABCF6EHQMjq27evHA7HOcOLaZr1hqLvrm/RooW+f/tiTU1NnX1atWrl99nhcOj06dMXUjqAEEToARCyOnXqpNTUVD3//PM6fvx4ne3V1dUaMGCADhw4oPLycmv97t275fF4FB8fL0nq2rWrKioq/PYtKSlpcD2tWrVSbW1tg/cDEBoIPQBC2sKFC1VbW6shQ4Zo5cqV2rt3r0pLSzV//nwlJydr7NixSkxM1K233qpt27apuLhYt912m0aNGqXBgwdLkq688kr985//1Msvv6y9e/fq8ccf165duxpcy2WXXaaCggK53W5VVVUF+lQBNDJCD4CQFhcXp23btmnMmDGaNWuWEhISNG7cOBUUFGjRokXWAwM7duyokSNHauzYserdu7dWrFhhHSM1NVWPPvqofvnLX+rHP/6xjh49qttuu63BtcybN0/r1q1TbGysBg4cGMjTBNAEeE4PAACwBUZ6AACALRB6AACALRB6AACALRB6AACALRB6AACALRB6AACALRB6AACALRB6AACALRB6AACALRB6AACALRB6AACALfx/e5HvKjKUW+oAAAAASUVORK5CYII=",
            "text/plain": [
              "<Figure size 640x480 with 1 Axes>"
            ]
          },
          "metadata": {
            
          },
          "output_type": "display_data"
        }
      ],
      "source": [
        "sns.countplot(y='opening_eco', \\\n",
        "              data = dfopeningeco, \\\n",
        "              orient='h', \\\n",
        "              order=dfopeningeco['opening_eco'].value_counts().index)\n",
        "plt.ylabel('Opening Eco')\n",
        "plt.xlabel('Count')\n",
        "plt.show()"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": 319,
      "id": "26faf020-ed43-4d51-9c38-a789d88942a7",
      "metadata": {
        "tags": [
          
        ]
      },
      "outputs": [
        {
          "data": {
            "text/plain": [
              "A00    0.050204\n",
              "C00    0.042078\n",
              "D00    0.036843\n",
              "B01    0.035696\n",
              "C41    0.034450\n",
              "         ...   \n",
              "A33    0.000050\n",
              "D22    0.000050\n",
              "E44    0.000050\n",
              "B58    0.000050\n",
              "D19    0.000050\n",
              "Name: opening_eco, Length: 365, dtype: float64"
            ]
          },
          "execution_count": 319,
          "metadata": {
            
          },
          "output_type": "execute_result"
        }
      ],
      "source": [
        "df.opening_eco.value_counts(normalize=True)"
      ]
    },
    {
      "cell_type": "markdown",
      "id": "54445bad-e536-4f2f-8e6e-5b993cdcfadd",
      "metadata": {
        
      },
      "source": [
        "As the plot shows, **A00** is the most winning **Opening Eco**, with 1007 games, representing 5.02% of the total count. "
      ]
    }
  ],
  "metadata": {
    "kernelspec": {
      "display_name": "Python 3 (ipykernel)",
      "language": "python",
      "name": "python3"
    },
    "language_info": {
      "codemirror_mode": {
        "name": "ipython",
        "version": 3
      },
      "file_extension": ".py",
      "mimetype": "text/x-python",
      "name": "python",
      "nbconvert_exporter": "python",
      "pygments_lexer": "ipython3",
      "version": "3.11.3"
    }
  },
  "nbformat": 4,
  "nbformat_minor": 5
}
