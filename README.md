# Placement-prediction-model
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "id": "66247863",
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import numpy as np\n",
    "import seaborn as sns\n",
    "import matplotlib.pyplot as plt\n",
    "import math\n",
    "from sklearn.model_selection import train_test_split\n",
    "from sklearn.linear_model import LogisticRegression\n",
    "from sklearn.linear_model import LinearRegression \n",
    "from sklearn.metrics import classification_report\n",
    "from sklearn.metrics import confusion_matrix\n",
    "\n",
    "data=pd.read_csv(\"Placement_Data_Full_Class.csv\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "id": "0eabc91a",
   "metadata": {},
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
       "      <th>sl_no</th>\n",
       "      <th>gender</th>\n",
       "      <th>ssc_p</th>\n",
       "      <th>ssc_b</th>\n",
       "      <th>hsc_p</th>\n",
       "      <th>hsc_b</th>\n",
       "      <th>hsc_s</th>\n",
       "      <th>degree_p</th>\n",
       "      <th>degree_t</th>\n",
       "      <th>workex</th>\n",
       "      <th>etest_p</th>\n",
       "      <th>specialisation</th>\n",
       "      <th>mba_p</th>\n",
       "      <th>status</th>\n",
       "      <th>salary</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>M</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>91.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>55.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>58.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>270000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>M</td>\n",
       "      <td>79.33</td>\n",
       "      <td>Central</td>\n",
       "      <td>78.33</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>77.48</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>Yes</td>\n",
       "      <td>86.5</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>66.28</td>\n",
       "      <td>Placed</td>\n",
       "      <td>200000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>M</td>\n",
       "      <td>65.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>68.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Arts</td>\n",
       "      <td>64.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>75.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>57.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>250000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>M</td>\n",
       "      <td>56.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>52.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Science</td>\n",
       "      <td>52.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>66.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>59.43</td>\n",
       "      <td>Not Placed</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>M</td>\n",
       "      <td>85.80</td>\n",
       "      <td>Central</td>\n",
       "      <td>73.60</td>\n",
       "      <td>Central</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>73.30</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>96.8</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>55.50</td>\n",
       "      <td>Placed</td>\n",
       "      <td>425000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>210</th>\n",
       "      <td>211</td>\n",
       "      <td>M</td>\n",
       "      <td>80.60</td>\n",
       "      <td>Others</td>\n",
       "      <td>82.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>77.60</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>91.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>74.49</td>\n",
       "      <td>Placed</td>\n",
       "      <td>400000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>211</th>\n",
       "      <td>212</td>\n",
       "      <td>M</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>60.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>72.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>74.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>53.62</td>\n",
       "      <td>Placed</td>\n",
       "      <td>275000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>212</th>\n",
       "      <td>213</td>\n",
       "      <td>M</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>73.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>Yes</td>\n",
       "      <td>59.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>69.72</td>\n",
       "      <td>Placed</td>\n",
       "      <td>295000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>213</th>\n",
       "      <td>214</td>\n",
       "      <td>F</td>\n",
       "      <td>74.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>66.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>70.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>60.23</td>\n",
       "      <td>Placed</td>\n",
       "      <td>204000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>214</th>\n",
       "      <td>215</td>\n",
       "      <td>M</td>\n",
       "      <td>62.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>53.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>89.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>60.22</td>\n",
       "      <td>Not Placed</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>215 rows × 15 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     sl_no gender  ssc_p    ssc_b  hsc_p    hsc_b     hsc_s  degree_p  \\\n",
       "0        1      M  67.00   Others  91.00   Others  Commerce     58.00   \n",
       "1        2      M  79.33  Central  78.33   Others   Science     77.48   \n",
       "2        3      M  65.00  Central  68.00  Central      Arts     64.00   \n",
       "3        4      M  56.00  Central  52.00  Central   Science     52.00   \n",
       "4        5      M  85.80  Central  73.60  Central  Commerce     73.30   \n",
       "..     ...    ...    ...      ...    ...      ...       ...       ...   \n",
       "210    211      M  80.60   Others  82.00   Others  Commerce     77.60   \n",
       "211    212      M  58.00   Others  60.00   Others   Science     72.00   \n",
       "212    213      M  67.00   Others  67.00   Others  Commerce     73.00   \n",
       "213    214      F  74.00   Others  66.00   Others  Commerce     58.00   \n",
       "214    215      M  62.00  Central  58.00   Others   Science     53.00   \n",
       "\n",
       "      degree_t workex  etest_p specialisation  mba_p      status    salary  \n",
       "0     Sci&Tech     No     55.0         Mkt&HR  58.80      Placed  270000.0  \n",
       "1     Sci&Tech    Yes     86.5        Mkt&Fin  66.28      Placed  200000.0  \n",
       "2    Comm&Mgmt     No     75.0        Mkt&Fin  57.80      Placed  250000.0  \n",
       "3     Sci&Tech     No     66.0         Mkt&HR  59.43  Not Placed       NaN  \n",
       "4    Comm&Mgmt     No     96.8        Mkt&Fin  55.50      Placed  425000.0  \n",
       "..         ...    ...      ...            ...    ...         ...       ...  \n",
       "210  Comm&Mgmt     No     91.0        Mkt&Fin  74.49      Placed  400000.0  \n",
       "211   Sci&Tech     No     74.0        Mkt&Fin  53.62      Placed  275000.0  \n",
       "212  Comm&Mgmt    Yes     59.0        Mkt&Fin  69.72      Placed  295000.0  \n",
       "213  Comm&Mgmt     No     70.0         Mkt&HR  60.23      Placed  204000.0  \n",
       "214  Comm&Mgmt     No     89.0         Mkt&HR  60.22  Not Placed       NaN  \n",
       "\n",
       "[215 rows x 15 columns]"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "id": "4b402474",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.heatmap(data.isnull(), cbar=False )"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "id": "974a9de4",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "sl_no              0\n",
       "gender             0\n",
       "ssc_p              0\n",
       "ssc_b              0\n",
       "hsc_p              0\n",
       "hsc_b              0\n",
       "hsc_s              0\n",
       "degree_p           0\n",
       "degree_t           0\n",
       "workex             0\n",
       "etest_p            0\n",
       "specialisation     0\n",
       "mba_p              0\n",
       "status             0\n",
       "salary            67\n",
       "dtype: int64"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "id": "166fef64",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 215 entries, 0 to 214\n",
      "Data columns (total 15 columns):\n",
      " #   Column          Non-Null Count  Dtype  \n",
      "---  ------          --------------  -----  \n",
      " 0   sl_no           215 non-null    int64  \n",
      " 1   gender          215 non-null    object \n",
      " 2   ssc_p           215 non-null    float64\n",
      " 3   ssc_b           215 non-null    object \n",
      " 4   hsc_p           215 non-null    float64\n",
      " 5   hsc_b           215 non-null    object \n",
      " 6   hsc_s           215 non-null    object \n",
      " 7   degree_p        215 non-null    float64\n",
      " 8   degree_t        215 non-null    object \n",
      " 9   workex          215 non-null    object \n",
      " 10  etest_p         215 non-null    float64\n",
      " 11  specialisation  215 non-null    object \n",
      " 12  mba_p           215 non-null    float64\n",
      " 13  status          215 non-null    object \n",
      " 14  salary          148 non-null    float64\n",
      "dtypes: float64(6), int64(1), object(8)\n",
      "memory usage: 25.3+ KB\n"
     ]
    }
   ],
   "source": [
    "data.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "id": "6be00ea6",
   "metadata": {},
   "outputs": [],
   "source": [
    "val=data['salary'].isnull()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "id": "0a248983",
   "metadata": {},
   "outputs": [],
   "source": [
    "data=data.replace(np.nan,0)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "id": "654f0d29",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.heatmap(data.isnull(), cbar=False )"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "id": "1a6c1b76",
   "metadata": {},
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
       "      <th>sl_no</th>\n",
       "      <th>gender</th>\n",
       "      <th>ssc_p</th>\n",
       "      <th>ssc_b</th>\n",
       "      <th>hsc_p</th>\n",
       "      <th>hsc_b</th>\n",
       "      <th>hsc_s</th>\n",
       "      <th>degree_p</th>\n",
       "      <th>degree_t</th>\n",
       "      <th>workex</th>\n",
       "      <th>etest_p</th>\n",
       "      <th>specialisation</th>\n",
       "      <th>mba_p</th>\n",
       "      <th>status</th>\n",
       "      <th>salary</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>M</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>91.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>55.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>58.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>270000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>M</td>\n",
       "      <td>79.33</td>\n",
       "      <td>Central</td>\n",
       "      <td>78.33</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>77.48</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>Yes</td>\n",
       "      <td>86.5</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>66.28</td>\n",
       "      <td>Placed</td>\n",
       "      <td>200000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>M</td>\n",
       "      <td>65.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>68.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Arts</td>\n",
       "      <td>64.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>75.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>57.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>250000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>M</td>\n",
       "      <td>56.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>52.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Science</td>\n",
       "      <td>52.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>66.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>59.43</td>\n",
       "      <td>Not Placed</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>M</td>\n",
       "      <td>85.80</td>\n",
       "      <td>Central</td>\n",
       "      <td>73.60</td>\n",
       "      <td>Central</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>73.30</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>96.8</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>55.50</td>\n",
       "      <td>Placed</td>\n",
       "      <td>425000.0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   sl_no gender  ssc_p    ssc_b  hsc_p    hsc_b     hsc_s  degree_p  \\\n",
       "0      1      M  67.00   Others  91.00   Others  Commerce     58.00   \n",
       "1      2      M  79.33  Central  78.33   Others   Science     77.48   \n",
       "2      3      M  65.00  Central  68.00  Central      Arts     64.00   \n",
       "3      4      M  56.00  Central  52.00  Central   Science     52.00   \n",
       "4      5      M  85.80  Central  73.60  Central  Commerce     73.30   \n",
       "\n",
       "    degree_t workex  etest_p specialisation  mba_p      status    salary  \n",
       "0   Sci&Tech     No     55.0         Mkt&HR  58.80      Placed  270000.0  \n",
       "1   Sci&Tech    Yes     86.5        Mkt&Fin  66.28      Placed  200000.0  \n",
       "2  Comm&Mgmt     No     75.0        Mkt&Fin  57.80      Placed  250000.0  \n",
       "3   Sci&Tech     No     66.0         Mkt&HR  59.43  Not Placed       0.0  \n",
       "4  Comm&Mgmt     No     96.8        Mkt&Fin  55.50      Placed  425000.0  "
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "id": "7c4d6b08",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:xlabel='status', ylabel='count'>"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.countplot(x='status',data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "id": "a2e56f27",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:xlabel='status', ylabel='count'>"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.countplot(x='status',hue='gender',data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "id": "00feb416",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:ylabel='Frequency'>"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "data['degree_p'].plot.hist()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "id": "33602b32",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:xlabel='status', ylabel='count'>"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAX4AAAEGCAYAAABiq/5QAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuNCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8QVMy6AAAACXBIWXMAAAsTAAALEwEAmpwYAAAdLElEQVR4nO3de5xVdf3v8debS4wJKMhAGD8CDUxkAGVAEQ+CKGppGiZoqeCNU/nD9Hek+B3LzEuRl/Jaj7BUNEXMS3L0cTwqihcyZMAhUDRSQVGCASHBO+Pn/LEX48AwuHFm7c3Mej8fDx97r+9el88eN+/5znev9V2KCMzMLDtaFLsAMzMrLAe/mVnGOPjNzDLGwW9mljEOfjOzjGlV7ALy0alTp+jRo0exyzAza1Lmz5+/JiJKt25vEsHfo0cPKioqil2GmVmTImn5tto91GNmljEOfjOzjHHwm5llTKpj/JLOB84CAlgEnA58EZgB9ACWAWMiYl2adZhZ8Xz88cesWLGCDz74oNilNFslJSV069aN1q1b57V+asEv6cvAuUCfiHhf0t3ASUAfYFZETJE0GZgM/DitOsysuFasWEG7du3o0aMHkopdTrMTEaxdu5YVK1bQs2fPvLZJe6inFbCLpFbkevpvAccB05LXpwHHp1yDmRXRBx98wB577OHQT4kk9thjjx36iyq14I+IN4GrgNeBlcC/I+IRoEtErEzWWQl03tb2kiZIqpBUUVVVlVaZZlYADv107ejPN7Xgl9SBXO++J7AnsKukU/LdPiKmRkR5RJSXlta5/sDMzD6nNId6Dgdei4iqiPgYuA84GFglqStA8rg6xRrMzGwraZ7V8zpwkKQvAu8DI4EK4F1gHDAleXwgxRpqDJx0WyEO02jmX3lasUswaxaWLVvGMcccw+LFi4tdyk4jteCPiLmS7gEWAJuA54GpQFvgbklnkvvlcGJaNZiZWV2pntUTET+LiK9FRN+IODUiPoyItRExMiJ6JY9vp1mDmVl1dTVnn302++23H6NGjeL999/nuuuuo0+fPvTr14+TTjoJgI0bN3L66adTVlZGv379uPfee+vd3/jx4+nbty9lZWX85je/KeTbabAmMUmbmVlDLF26lOnTp3PTTTcxZswY7r33XqZMmcJrr71GmzZtWL9+PQCXXnopu+22G4sWLQJg3bptX1taWVnJm2++WTN8tHn7psJTNphZs9ezZ08GDBgAwMCBA1m2bBn9+vXju9/9Ln/6059o1SrXB37sscc455xzarbr0KHDNve311578eqrrzJx4kQefvhh2rdvn/p7aEwOfjNr9tq0aVPzvGXLlmzatImHHnqIc845h/nz5zNw4EA2bdpEROR1TnyHDh1YuHAhw4cP58Ybb+Sss85Ks/xG5+A3s8z55JNPeOONNxgxYgRXXHEF69evZ+PGjYwaNYobbrihZr36hnrWrFnDJ598wgknnMCll17KggULClV6o3Dwm1nmVFdXc8opp1BWVsb+++/P+eefz+67785PfvIT1q1bR9++fenfvz9PPPHENrd/8803GT58OAMGDGD8+PH88pe/LPA7aBh/uWtmzVqPHj22OIf/ggsuqHfdtm3bMm3atHpf36x///5Nrpdfm3v8ZmYZ4x6/mdl2HHjggXz44YdbtN1+++2UlZUVqaKGc/CbmW3H3Llzi11Co/NQj5lZxjj4zcwyxsFvZpYxHuM3s4Jq7CnS853C/F//+hfnnXce8+bNo02bNvTo0YNrrrmG3r17N2o9TYF7/GbW7EUE3/rWtxg+fDivvPIKL774Ir/4xS9YtWpVsUurUV1dXbBjOfjNrNl74oknaN26Nd/73vdq2gYMGMAhhxzCpEmTaqZXnjFjBgCzZ8/m0EMPZcyYMfTu3ZvJkydzxx13MHjwYMrKynjllVcAGD9+PN///vcZMWIEe+21F08++SRnnHEG++67L+PHj6851iOPPMKQIUM44IADOPHEE9m4cSOQu7jskksu4ZBDDuHPf/4zDz/8MAcccAD9+/dn5MiRALz77rucccYZDBo0iP33358HHmj4vas81GNmzd7ixYsZOHBgnfb77ruPyspKFi5cyJo1axg0aBDDhg0DYOHChSxZsoSOHTuy1157cdZZZ/Hcc89x7bXXcv3113PNNdcAufl8Hn/8cWbOnMmxxx7LnDlz+MMf/sCgQYOorKykW7duXHbZZTz22GPsuuuu/OpXv+LXv/41F110EQAlJSU888wzVFVVccABB/DUU0/Rs2dP3n47d6uSyy+/nMMOO4ybb76Z9evXM3jwYA4//HB23XXXz/3zcPCbWWY988wznHzyybRs2ZIuXbpw6KGHMm/ePNq3b8+gQYPo2rUrAHvvvTejRo0CoKysbIs5fI49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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.countplot(x='status',hue='hsc_s',data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "id": "b062fa77",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:xlabel='status', ylabel='count'>"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.countplot(x='status',hue='workex',data=data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "id": "54721d8b",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:ylabel='Frequency'>"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAX4AAAD4CAYAAADrRI2NAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuNCwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8QVMy6AAAACXBIWXMAAAsTAAALEwEAmpwYAAAPyklEQVR4nO3df5BdZX3H8feHRISglKQsNAK60GGw6NSCaytiOyraVlDQP2hxaie1KJ3RWn90xgbtFPqHM2gtg9aOErVOxB8VkAKFWo2p2namBRahAgYaBIyRlKx2KkodEf32j3tSNskmufvj7HX3eb9m7txznnvPOd/zzO5nzz7n3HNTVUiS2nHQqAuQJC0ug1+SGmPwS1JjDH5JaozBL0mNWTnqAoZx5JFH1vj4+KjLkKQl5dZbb/12VY3t2b4kgn98fJzJyclRlyFJS0qSb8zU7lCPJDXG4Jekxhj8ktQYg1+SGmPwS1JjDH5JaozBL0mNMfglqTEGvyQ1Zkl8clezM77+xpFs94FLzhrJdiXNjkf8ktQYg1+SGmPwS1JjDH5JaozBL0mNMfglqTEGvyQ1xuCXpMYY/JLUGINfkhpj8EtSYwx+SWqMwS9JjTH4JakxBr8kNcbgl6TGGPyS1BiDX5IaY/BLUmMMfklqjMEvSY0x+CWpMQa/JDXG4Jekxhj8ktQYg1+SGtNr8Cd5S5K7ktyZ5FNJDkmyJsmmJFu759V91iBJ2l1vwZ/kGOCPgImqeiawAjgPWA9srqoTgc3dvCRpkfQ91LMSODTJSmAV8CBwDrCxe30j8Iqea5AkTdNb8FfVt4D3ANuAHcB3q+rzwNFVtaN7zw7gqL5qkCTtrc+hntUMju6PB54CHJbk1bNY/oIkk0kmp6am+ipTkprT51DPi4H7q2qqqn4EXAM8D3goyVqA7nnnTAtX1YaqmqiqibGxsR7LlKS29Bn824DnJlmVJMAZwBbgemBd9551wHU91iBJ2sPKvlZcVTcluRr4CvAYcBuwAXgScGWS8xn8cTi3rxokSXvrLfgBquoi4KI9mn/I4OhfkjQCfnJXkhpj8EtSYwx+SWqMwS9JjTH4JakxBr8kNcbgl6TGGPyS1BiDX5IaY/BLUmMMfklqjMEvSY0x+CWpMQa/JDXG4Jekxhj8ktQYg1+SGmPwS1JjDH5JaozBL0mNMfglqTEGvyQ1xuCXpMasHHUBfRtff+PItv3AJWeNbNuStC8e8UtSYwx+SWqMwS9JjTH4JakxBr8kNcbgl6TGGPyS1BiDX5IaY/BLUmMMfklqjMEvSY3pNfiTHJHk6iR3J9mS5LQka5JsSrK1e17dZw2SpN31fcT/XuAfq+rpwLOALcB6YHNVnQhs7uYlSYukt+BPcjjwa8BHAKrq0ar6H+AcYGP3to3AK/qqQZK0tz6P+E8ApoCPJrktyYeTHAYcXVU7ALrno3qsQZK0h6GCP8kz57DulcCpwAeq6hTgEWYxrJPkgiSTSSanpqbmsHlJ0kyGPeL/YJKbk7w+yRFDLrMd2F5VN3XzVzP4Q/BQkrUA3fPOmRauqg1VNVFVE2NjY0NuUpJ0IEMFf1U9H/gd4DhgMsknk7zkAMv8F/DNJCd1TWcAXwOuB9Z1beuA6+ZSuCRpbob+6sWq2prkT4FJ4H3AKUkCvL2qrtnHYm8EPpHkYOA+4DUM/thcmeR8YBtw7nx2QJI0O0MFf5JfZBDaZwGbgJdX1VeSPAX4N2DG4K+q24GJGV46Y07VSpLmbdgj/vcDH2JwdP+DXY1V9WD3X4AkaYkYNvjPBH5QVT8GSHIQcEhV/W9VXdFbdZKkBTfsVT1fAA6dNr+qa5MkLTHDBv8hVfX9XTPd9Kp+SpIk9WnY4H8kyam7ZpI8G/jBft4vSfopNewY/5uBq5I82M2vBX67l4okSb0aKvir6pYkTwdOAgLcXVU/6rUySVIvhv4AF/AcYLxb5pQkVNXHeqlKktSbYT/AdQXw88DtwI+75gIMfklaYoY94p8ATq6q6rMYSVL/hr2q507g5/osRJK0OIY94j8S+FqSm4Ef7mqsqrN7qUqS1Jthg//iPouQJC2eYS/n/HKSpwEnVtUXkqwCVvRbmiSpD8N+9eLrGHyD1uVd0zHAtT3VJEnq0bAnd98AnA48DIMvZcEvSZekJWnY4P9hVT26aybJSgbX8UuSlphhg//LSd4OHNp91+5VwN/3V5YkqS/DBv96YAq4A/gD4B8Av3lLkpagYa/q+QmDr178UL/lSJL6Nuy9eu5nhjH9qjphwSuSJPVqNvfq2eUQ4FxgzcKXI0nq21Bj/FX1nWmPb1XVZcCL+i1NktSHYYd6Tp02exCD/wCe3EtFkqReDTvU85fTph8DHgB+a8GrkST1btirel7YdyGSpMUx7FDPW/f3elVdujDlSJL6Npurep4DXN/Nvxz4Z+CbfRQlSerPbL6I5dSq+h5AkouBq6rqtX0VJknqx7C3bHgq8Oi0+UeB8QWvRpLUu2GP+K8Abk7ydww+wftK4GO9VSVJ6s2wV/W8M8lngV/tml5TVbf1V5YkqS/DDvUArAIerqr3AtuTHN9TTZKkHg371YsXAX8CXNg1PQH4eF9FSZL6M+wR/yuBs4FHAKrqQbxlgyQtScMG/6NVVXS3Zk5yWH8lSZL6NGzwX5nkcuCIJK8DvsCQX8qSZEWS25Lc0M2vSbIpydbuefXcSpckzcUBgz9JgE8DVwOfAU4C/qyq/mrIbbwJ2DJtfj2wuapOBDZ385KkRXLAyzmrqpJcW1XPBjbNZuVJjgXOAt4J7LrfzznAC7rpjcCXGJw4liQtgmGHev49yXPmsP7LgLcBP5nWdnRV7QDono+aacEkFySZTDI5NTU1h01LkmYybPC/kEH4fz3JV5PckeSr+1sgycuAnVV161wKq6oNVTVRVRNjY2NzWYUkaQb7HepJ8tSq2ga8dA7rPh04O8mZDL6n9/AkHwceSrK2qnYkWQvsnMO6JUlzdKAj/msBquobwKVV9Y3pj/0tWFUXVtWxVTUOnAf8U1W9msGtndd1b1sHXDefHZAkzc6BTu5m2vQJC7TNSxhcHno+sA04d4HWqxEbX3/jyLb9wCVnjWzb0lJzoOCvfUzPSlV9icHVO1TVd4Az5rouSdL8HCj4n5XkYQZH/od203TzVVWH91qdJGnB7Tf4q2rFYhUiSVocs7ktsyRpGTD4JakxBr8kNcbgl6TGGPyS1BiDX5IaY/BLUmMMfklqjMEvSY0x+CWpMQa/JDXG4Jekxhj8ktQYg1+SGmPwS1JjDH5JaozBL0mNMfglqTEGvyQ1xuCXpMYY/JLUGINfkhpj8EtSYwx+SWqMwS9JjTH4JakxBr8kNcbgl6TGGPyS1BiDX5IaY/BLUmMMfklqjMEvSY3pLfiTHJfki0m2JLkryZu69jVJNiXZ2j2v7qsGSdLe+jzifwz446r6BeC5wBuSnAysBzZX1YnA5m5ekrRIegv+qtpRVV/ppr8HbAGOAc4BNnZv2wi8oq8aJEl7W5Qx/iTjwCnATcDRVbUDBn8cgKP2scwFSSaTTE5NTS1GmZLUhN6DP8mTgM8Ab66qh4ddrqo2VNVEVU2MjY31V6AkNabX4E/yBAah/4mquqZrfijJ2u71tcDOPmuQJO2uz6t6AnwE2FJVl0576XpgXTe9DriurxokSXtb2eO6Twd+F7gjye1d29uBS4Ark5wPbAPO7bEGSdIeegv+qvpXIPt4+Yy+titJ2j8/uStJjTH4JakxBr8kNcbgl6TGGPyS1BiDX5IaY/BLUmMMfklqjMEvSY0x+CWpMQa/JDXG4Jekxhj8ktQYg1+SGmPwS1JjDH5Jakyf38AlLZrx9TeOZLsPXHLWSLYrzYdH/JLUGINfkhpj8EtSYwx+SWqMwS9JjTH4JakxBr8kNcbgl6TGGPyS1BiDX5IaY/BLUmMMfklqjMEvSY0x+CWpMd6WWZqHUd0OGrwltObOI35JaozBL0mNMfglqTEjCf4kv5nkniT3Jlk/ihokqVWLfnI3yQrgr4GXANuBW5JcX1VfW+xapKVslCeWR2VUJ7SX20n8URzx/zJwb1XdV1WPAn8LnDOCOiSpSaO4nPMY4JvT5rcDv7Lnm5JcAFzQzX4/yT1z3N6RwLfnuOy85F2j2OqMRtYHP0Va74Nlsf/z/J1akn0wz31+2kyNowj+zNBWezVUbQA2zHtjyWRVTcx3PUuZfWAftL7/YB9MN4qhnu3AcdPmjwUeHEEdktSkUQT/LcCJSY5PcjBwHnD9COqQpCYt+lBPVT2W5A+BzwErgL+pqrt63OS8h4uWAfvAPmh9/8E++H+p2mt4XZK0jPnJXUlqjMEvSY1Z1sG/1G8NkeS4JF9MsiXJXUne1LWvSbIpydbuefW0ZS7s9veeJL8xrf3ZSe7oXntfknTtT0zy6a79piTj05ZZ121ja5J1i7jru0myIsltSW7o5lvb/yOSXJ3k7u5n4bQG++At3e/AnUk+leSQ1vpgQVXVsnwwOHH8deAE4GDgP4CTR13XLPdhLXBqN/1k4D+Bk4F3A+u79vXAu7rpk7v9fCJwfLf/K7rXbgZOY/A5is8CL+3aXw98sJs+D/h0N70GuK97Xt1Nrx5RP7wV+CRwQzff2v5vBF7bTR8MHNFSHzD40Of9wKHd/JXA77XUBwvep6MuoMcfltOAz02bvxC4cNR1zXOfrmNwj6N7gLVd21rgnpn2kcGVU6d177l7WvurgMunv6ebXsngk42Z/p7utcuBV41gn48FNgMv4vHgb2n/D+9CL3u0t9QHuz7tv6ar7wbg11vqg4V+LOehnpluDXHMiGqZt+5fz1OAm4Cjq2oHQPd8VPe2fe3zMd30nu27LVNVjwHfBX52P+tabJcBbwN+Mq2tpf0/AZgCPtoNd304yWE01AdV9S3gPcA2YAfw3ar6PA31wUJbzsE/1K0hloIkTwI+A7y5qh7e31tnaKv9tM91mUWR5GXAzqq6ddhFZmhbsvvfWQmcCnygqk4BHmEwrLEvy64PurH7cxgM2zwFOCzJq/e3yAxtS7oPFtpyDv5lcWuIJE9gEPqfqKpruuaHkqztXl8L7Oza97XP27vpPdt3WybJSuBngP/ez7oW0+nA2UkeYHAX1xcl+Tjt7D9dHdur6qZu/moGfwha6oMXA/dX1VRV/Qi4BngebfXBwhr1WFNfDwZHSvcxOErYdXL3GaOua5b7EOBjwGV7tP8Fu5/Uenc3/Qx2P6l1H4+f1LoFeC6Pn9Q6s2t/A7uf1Lqym17DYGx5dfe4H1gzwr54AY+P8Te1/8C/ACd10xd3+99MHzC4e+9dwKqu9o3AG1vqgwXv01EX0PMPzJkMroT5OvCOUdczh/qfz+Dfyq8Ct3ePMxmMPW4GtnbPa6Yt845uf++hu2Kha58A7uxeez+Pf2r7EOAq4F4GVzycMG2Z3+/a7wVeM+K+eAGPB39T+w/8EjDZ/Rxc2wVQa33w58DdXf1XMAj1pvpgIR/eskGSGrOcx/glSTMw+CWpMQa/JDXG4Jekxhj8ktQYg1+SGmPwS1Jj/g8sYhLpKrYyagAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "data['salary'].plot.hist()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "id": "608b0f76",
   "metadata": {},
   "outputs": [],
   "source": [
    "gen=pd.get_dummies(data['gender'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "id": "ec6ee32f",
   "metadata": {},
   "outputs": [],
   "source": [
    "ssc_b=pd.get_dummies(data['ssc_b'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "id": "9c5ae1ec",
   "metadata": {},
   "outputs": [],
   "source": [
    "hsc_s=pd.get_dummies(data['hsc_s'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "id": "6b0a934d",
   "metadata": {},
   "outputs": [],
   "source": [
    "hsc_b=pd.get_dummies(data['hsc_b'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "id": "f4260f86",
   "metadata": {},
   "outputs": [],
   "source": [
    "deg=pd.get_dummies(data['degree_t'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "id": "9e738bd0",
   "metadata": {},
   "outputs": [],
   "source": [
    "work=pd.get_dummies(data['workex'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "id": "758d3394",
   "metadata": {},
   "outputs": [],
   "source": [
    "spec=pd.get_dummies(data['specialisation'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "id": "2aef3c8c",
   "metadata": {},
   "outputs": [],
   "source": [
    "y=pd.get_dummies(data['status'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "id": "65d1a3ac",
   "metadata": {},
   "outputs": [],
   "source": [
    "X=data.drop(['sl_no','gender','ssc_b','hsc_s','hsc_b','degree_t','workex','specialisation','salary','status'],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "id": "14ad6197",
   "metadata": {
    "scrolled": true
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
       "      <th>ssc_p</th>\n",
       "      <th>hsc_p</th>\n",
       "      <th>degree_p</th>\n",
       "      <th>etest_p</th>\n",
       "      <th>mba_p</th>\n",
       "      <th>M</th>\n",
       "      <th>Others</th>\n",
       "      <th>Commerce</th>\n",
       "      <th>Science</th>\n",
       "      <th>Others</th>\n",
       "      <th>Others</th>\n",
       "      <th>Sci&amp;Tech</th>\n",
       "      <th>Yes</th>\n",
       "      <th>Mkt&amp;HR</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>67.00</td>\n",
       "      <td>91.00</td>\n",
       "      <td>58.00</td>\n",
       "      <td>55.0</td>\n",
       "      <td>58.80</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>79.33</td>\n",
       "      <td>78.33</td>\n",
       "      <td>77.48</td>\n",
       "      <td>86.5</td>\n",
       "      <td>66.28</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>65.00</td>\n",
       "      <td>68.00</td>\n",
       "      <td>64.00</td>\n",
       "      <td>75.0</td>\n",
       "      <td>57.80</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>56.00</td>\n",
       "      <td>52.00</td>\n",
       "      <td>52.00</td>\n",
       "      <td>66.0</td>\n",
       "      <td>59.43</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>85.80</td>\n",
       "      <td>73.60</td>\n",
       "      <td>73.30</td>\n",
       "      <td>96.8</td>\n",
       "      <td>55.50</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>210</th>\n",
       "      <td>80.60</td>\n",
       "      <td>82.00</td>\n",
       "      <td>77.60</td>\n",
       "      <td>91.0</td>\n",
       "      <td>74.49</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>211</th>\n",
       "      <td>58.00</td>\n",
       "      <td>60.00</td>\n",
       "      <td>72.00</td>\n",
       "      <td>74.0</td>\n",
       "      <td>53.62</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>212</th>\n",
       "      <td>67.00</td>\n",
       "      <td>67.00</td>\n",
       "      <td>73.00</td>\n",
       "      <td>59.0</td>\n",
       "      <td>69.72</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>213</th>\n",
       "      <td>74.00</td>\n",
       "      <td>66.00</td>\n",
       "      <td>58.00</td>\n",
       "      <td>70.0</td>\n",
       "      <td>60.23</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>214</th>\n",
       "      <td>62.00</td>\n",
       "      <td>58.00</td>\n",
       "      <td>53.00</td>\n",
       "      <td>89.0</td>\n",
       "      <td>60.22</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>215 rows × 14 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     ssc_p  hsc_p  degree_p  etest_p  mba_p  M  Others  Commerce  Science  \\\n",
       "0    67.00  91.00     58.00     55.0  58.80  1       1         1        0   \n",
       "1    79.33  78.33     77.48     86.5  66.28  1       0         0        1   \n",
       "2    65.00  68.00     64.00     75.0  57.80  1       0         0        0   \n",
       "3    56.00  52.00     52.00     66.0  59.43  1       0         0        1   \n",
       "4    85.80  73.60     73.30     96.8  55.50  1       0         1        0   \n",
       "..     ...    ...       ...      ...    ... ..     ...       ...      ...   \n",
       "210  80.60  82.00     77.60     91.0  74.49  1       1         1        0   \n",
       "211  58.00  60.00     72.00     74.0  53.62  1       1         0        1   \n",
       "212  67.00  67.00     73.00     59.0  69.72  1       1         1        0   \n",
       "213  74.00  66.00     58.00     70.0  60.23  0       1         1        0   \n",
       "214  62.00  58.00     53.00     89.0  60.22  1       0         0        1   \n",
       "\n",
       "     Others  Others  Sci&Tech  Yes  Mkt&HR  \n",
       "0         1       0         1    0       1  \n",
       "1         1       0         1    1       0  \n",
       "2         0       0         0    0       0  \n",
       "3         0       0         1    0       1  \n",
       "4         0       0         0    0       0  \n",
       "..      ...     ...       ...  ...     ...  \n",
       "210       1       0         0    0       0  \n",
       "211       1       0         1    0       0  \n",
       "212       1       0         0    1       0  \n",
       "213       1       0         0    0       1  \n",
       "214       1       0         0    0       1  \n",
       "\n",
       "[215 rows x 14 columns]"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pd.concat([X,gen,ssc_b,hsc_s,hsc_b,deg,work,spec],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "id": "5f423631",
   "metadata": {},
   "outputs": [],
   "source": [
    "y=y['Placed']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "id": "0db90d2d",
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=2)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "id": "e314f032",
   "metadata": {},
   "outputs": [],
   "source": [
    "lm=LogisticRegression(max_iter=10000)\n",
    "lm.fit(X_train,y_train)\n",
    "y_predict=lm.predict(X_test)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "id": "8485dbec",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([[13,  5],\n",
       "       [10, 37]], dtype=int64)"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "confusion_matrix(y_predict,y_test)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "id": "c0edc4e1",
   "metadata": {},
   "outputs": [],
   "source": [
    "xp=X_test.index\n",
    "yt=y_test\n",
    "yp=y_predict"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "id": "60d73691",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.plot(xp,yt,'o',label='act')\n",
    "plt.plot(xp,yp,'o',label='pred')\n",
    "plt.legend()\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "id": "e850e32d",
   "metadata": {},
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
       "      <th>sl_no</th>\n",
       "      <th>gender</th>\n",
       "      <th>ssc_p</th>\n",
       "      <th>ssc_b</th>\n",
       "      <th>hsc_p</th>\n",
       "      <th>hsc_b</th>\n",
       "      <th>hsc_s</th>\n",
       "      <th>degree_p</th>\n",
       "      <th>degree_t</th>\n",
       "      <th>workex</th>\n",
       "      <th>etest_p</th>\n",
       "      <th>specialisation</th>\n",
       "      <th>mba_p</th>\n",
       "      <th>status</th>\n",
       "      <th>salary</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>M</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>91.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>55.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>58.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>270000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>M</td>\n",
       "      <td>79.33</td>\n",
       "      <td>Central</td>\n",
       "      <td>78.33</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>77.48</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>Yes</td>\n",
       "      <td>86.5</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>66.28</td>\n",
       "      <td>Placed</td>\n",
       "      <td>200000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>M</td>\n",
       "      <td>65.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>68.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Arts</td>\n",
       "      <td>64.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>75.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>57.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>250000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>M</td>\n",
       "      <td>56.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>52.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Science</td>\n",
       "      <td>52.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>66.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>59.43</td>\n",
       "      <td>Not Placed</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>M</td>\n",
       "      <td>85.80</td>\n",
       "      <td>Central</td>\n",
       "      <td>73.60</td>\n",
       "      <td>Central</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>73.30</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>96.8</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>55.50</td>\n",
       "      <td>Placed</td>\n",
       "      <td>425000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>210</th>\n",
       "      <td>211</td>\n",
       "      <td>M</td>\n",
       "      <td>80.60</td>\n",
       "      <td>Others</td>\n",
       "      <td>82.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>77.60</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>91.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>74.49</td>\n",
       "      <td>Placed</td>\n",
       "      <td>400000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>211</th>\n",
       "      <td>212</td>\n",
       "      <td>M</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>60.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>72.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>74.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>53.62</td>\n",
       "      <td>Placed</td>\n",
       "      <td>275000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>212</th>\n",
       "      <td>213</td>\n",
       "      <td>M</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>73.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>Yes</td>\n",
       "      <td>59.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>69.72</td>\n",
       "      <td>Placed</td>\n",
       "      <td>295000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>213</th>\n",
       "      <td>214</td>\n",
       "      <td>F</td>\n",
       "      <td>74.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>66.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>70.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>60.23</td>\n",
       "      <td>Placed</td>\n",
       "      <td>204000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>214</th>\n",
       "      <td>215</td>\n",
       "      <td>M</td>\n",
       "      <td>62.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>53.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>89.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>60.22</td>\n",
       "      <td>Not Placed</td>\n",
       "      <td>0.0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>215 rows × 15 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     sl_no gender  ssc_p    ssc_b  hsc_p    hsc_b     hsc_s  degree_p  \\\n",
       "0        1      M  67.00   Others  91.00   Others  Commerce     58.00   \n",
       "1        2      M  79.33  Central  78.33   Others   Science     77.48   \n",
       "2        3      M  65.00  Central  68.00  Central      Arts     64.00   \n",
       "3        4      M  56.00  Central  52.00  Central   Science     52.00   \n",
       "4        5      M  85.80  Central  73.60  Central  Commerce     73.30   \n",
       "..     ...    ...    ...      ...    ...      ...       ...       ...   \n",
       "210    211      M  80.60   Others  82.00   Others  Commerce     77.60   \n",
       "211    212      M  58.00   Others  60.00   Others   Science     72.00   \n",
       "212    213      M  67.00   Others  67.00   Others  Commerce     73.00   \n",
       "213    214      F  74.00   Others  66.00   Others  Commerce     58.00   \n",
       "214    215      M  62.00  Central  58.00   Others   Science     53.00   \n",
       "\n",
       "      degree_t workex  etest_p specialisation  mba_p      status    salary  \n",
       "0     Sci&Tech     No     55.0         Mkt&HR  58.80      Placed  270000.0  \n",
       "1     Sci&Tech    Yes     86.5        Mkt&Fin  66.28      Placed  200000.0  \n",
       "2    Comm&Mgmt     No     75.0        Mkt&Fin  57.80      Placed  250000.0  \n",
       "3     Sci&Tech     No     66.0         Mkt&HR  59.43  Not Placed       0.0  \n",
       "4    Comm&Mgmt     No     96.8        Mkt&Fin  55.50      Placed  425000.0  \n",
       "..         ...    ...      ...            ...    ...         ...       ...  \n",
       "210  Comm&Mgmt     No     91.0        Mkt&Fin  74.49      Placed  400000.0  \n",
       "211   Sci&Tech     No     74.0        Mkt&Fin  53.62      Placed  275000.0  \n",
       "212  Comm&Mgmt    Yes     59.0        Mkt&Fin  69.72      Placed  295000.0  \n",
       "213  Comm&Mgmt     No     70.0         Mkt&HR  60.23      Placed  204000.0  \n",
       "214  Comm&Mgmt     No     89.0         Mkt&HR  60.22  Not Placed       0.0  \n",
       "\n",
       "[215 rows x 15 columns]"
      ]
     },
     "execution_count": 32,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "id": "8005d22f",
   "metadata": {},
   "outputs": [],
   "source": [
    "data2=pd.read_csv('Placement_Data_Full_Class.csv')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "id": "20406360",
   "metadata": {},
   "outputs": [],
   "source": [
    "data2=data2.dropna()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "id": "810b79be",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<AxesSubplot:>"
      ]
     },
     "execution_count": 35,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.heatmap(data2.isnull(),cbar=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "id": "a8a13ae6",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "sl_no             0\n",
       "gender            0\n",
       "ssc_p             0\n",
       "ssc_b             0\n",
       "hsc_p             0\n",
       "hsc_b             0\n",
       "hsc_s             0\n",
       "degree_p          0\n",
       "degree_t          0\n",
       "workex            0\n",
       "etest_p           0\n",
       "specialisation    0\n",
       "mba_p             0\n",
       "status            0\n",
       "salary            0\n",
       "dtype: int64"
      ]
     },
     "execution_count": 36,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data2.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "id": "e3aa4f72",
   "metadata": {},
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
       "      <th>sl_no</th>\n",
       "      <th>gender</th>\n",
       "      <th>ssc_p</th>\n",
       "      <th>ssc_b</th>\n",
       "      <th>hsc_p</th>\n",
       "      <th>hsc_b</th>\n",
       "      <th>hsc_s</th>\n",
       "      <th>degree_p</th>\n",
       "      <th>degree_t</th>\n",
       "      <th>workex</th>\n",
       "      <th>etest_p</th>\n",
       "      <th>specialisation</th>\n",
       "      <th>mba_p</th>\n",
       "      <th>status</th>\n",
       "      <th>salary</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>M</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>91.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>55.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>58.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>270000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>M</td>\n",
       "      <td>79.33</td>\n",
       "      <td>Central</td>\n",
       "      <td>78.33</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>77.48</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>Yes</td>\n",
       "      <td>86.5</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>66.28</td>\n",
       "      <td>Placed</td>\n",
       "      <td>200000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>M</td>\n",
       "      <td>65.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>68.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Arts</td>\n",
       "      <td>64.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>75.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>57.80</td>\n",
       "      <td>Placed</td>\n",
       "      <td>250000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>M</td>\n",
       "      <td>85.80</td>\n",
       "      <td>Central</td>\n",
       "      <td>73.60</td>\n",
       "      <td>Central</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>73.30</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>96.8</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>55.50</td>\n",
       "      <td>Placed</td>\n",
       "      <td>425000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>8</td>\n",
       "      <td>M</td>\n",
       "      <td>82.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>64.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Science</td>\n",
       "      <td>66.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>Yes</td>\n",
       "      <td>67.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>62.14</td>\n",
       "      <td>Placed</td>\n",
       "      <td>252000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>209</th>\n",
       "      <td>210</td>\n",
       "      <td>M</td>\n",
       "      <td>62.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>72.00</td>\n",
       "      <td>Central</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>65.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>67.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>56.49</td>\n",
       "      <td>Placed</td>\n",
       "      <td>216000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>210</th>\n",
       "      <td>211</td>\n",
       "      <td>M</td>\n",
       "      <td>80.60</td>\n",
       "      <td>Others</td>\n",
       "      <td>82.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>77.60</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>91.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>74.49</td>\n",
       "      <td>Placed</td>\n",
       "      <td>400000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>211</th>\n",
       "      <td>212</td>\n",
       "      <td>M</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>60.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Science</td>\n",
       "      <td>72.00</td>\n",
       "      <td>Sci&amp;Tech</td>\n",
       "      <td>No</td>\n",
       "      <td>74.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>53.62</td>\n",
       "      <td>Placed</td>\n",
       "      <td>275000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>212</th>\n",
       "      <td>213</td>\n",
       "      <td>M</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>67.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>73.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>Yes</td>\n",
       "      <td>59.0</td>\n",
       "      <td>Mkt&amp;Fin</td>\n",
       "      <td>69.72</td>\n",
       "      <td>Placed</td>\n",
       "      <td>295000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>213</th>\n",
       "      <td>214</td>\n",
       "      <td>F</td>\n",
       "      <td>74.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>66.00</td>\n",
       "      <td>Others</td>\n",
       "      <td>Commerce</td>\n",
       "      <td>58.00</td>\n",
       "      <td>Comm&amp;Mgmt</td>\n",
       "      <td>No</td>\n",
       "      <td>70.0</td>\n",
       "      <td>Mkt&amp;HR</td>\n",
       "      <td>60.23</td>\n",
       "      <td>Placed</td>\n",
       "      <td>204000.0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>148 rows × 15 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     sl_no gender  ssc_p    ssc_b  hsc_p    hsc_b     hsc_s  degree_p  \\\n",
       "0        1      M  67.00   Others  91.00   Others  Commerce     58.00   \n",
       "1        2      M  79.33  Central  78.33   Others   Science     77.48   \n",
       "2        3      M  65.00  Central  68.00  Central      Arts     64.00   \n",
       "4        5      M  85.80  Central  73.60  Central  Commerce     73.30   \n",
       "7        8      M  82.00  Central  64.00  Central   Science     66.00   \n",
       "..     ...    ...    ...      ...    ...      ...       ...       ...   \n",
       "209    210      M  62.00  Central  72.00  Central  Commerce     65.00   \n",
       "210    211      M  80.60   Others  82.00   Others  Commerce     77.60   \n",
       "211    212      M  58.00   Others  60.00   Others   Science     72.00   \n",
       "212    213      M  67.00   Others  67.00   Others  Commerce     73.00   \n",
       "213    214      F  74.00   Others  66.00   Others  Commerce     58.00   \n",
       "\n",
       "      degree_t workex  etest_p specialisation  mba_p  status    salary  \n",
       "0     Sci&Tech     No     55.0         Mkt&HR  58.80  Placed  270000.0  \n",
       "1     Sci&Tech    Yes     86.5        Mkt&Fin  66.28  Placed  200000.0  \n",
       "2    Comm&Mgmt     No     75.0        Mkt&Fin  57.80  Placed  250000.0  \n",
       "4    Comm&Mgmt     No     96.8        Mkt&Fin  55.50  Placed  425000.0  \n",
       "7     Sci&Tech    Yes     67.0        Mkt&Fin  62.14  Placed  252000.0  \n",
       "..         ...    ...      ...            ...    ...     ...       ...  \n",
       "209  Comm&Mgmt     No     67.0        Mkt&Fin  56.49  Placed  216000.0  \n",
       "210  Comm&Mgmt     No     91.0        Mkt&Fin  74.49  Placed  400000.0  \n",
       "211   Sci&Tech     No     74.0        Mkt&Fin  53.62  Placed  275000.0  \n",
       "212  Comm&Mgmt    Yes     59.0        Mkt&Fin  69.72  Placed  295000.0  \n",
       "213  Comm&Mgmt     No     70.0         Mkt&HR  60.23  Placed  204000.0  \n",
       "\n",
       "[148 rows x 15 columns]"
      ]
     },
     "execution_count": 37,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data2"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "id": "02637145",
   "metadata": {},
   "outputs": [],
   "source": [
    "gen2=pd.get_dummies(data2['gender'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "id": "5e91dc98",
   "metadata": {},
   "outputs": [],
   "source": [
    "ssc_b2=pd.get_dummies(data2['ssc_b'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "id": "84c0d14c",
   "metadata": {},
   "outputs": [],
   "source": [
    "hsc_b2=pd.get_dummies(data2['hsc_b'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "id": "254b501c",
   "metadata": {},
   "outputs": [],
   "source": [
    "hsc_s2=pd.get_dummies(data2['hsc_s'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "id": "f980373c",
   "metadata": {},
   "outputs": [],
   "source": [
    "deg2=pd.get_dummies(data2['degree_t'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "id": "8fe58145",
   "metadata": {},
   "outputs": [],
   "source": [
    "work2=pd.get_dummies(data2['workex'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "id": "1e6cc33f",
   "metadata": {},
   "outputs": [],
   "source": [
    "spe2=pd.get_dummies(data2['specialisation'],drop_first=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "id": "b03487ad",
   "metadata": {},
   "outputs": [],
   "source": [
    "y2=data2['salary']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "id": "23f72417",
   "metadata": {},
   "outputs": [],
   "source": [
    "X2=data2.drop(['sl_no','gender','ssc_b','hsc_s','hsc_b','degree_t','workex','specialisation','salary','status'],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "id": "850cccbe",
   "metadata": {},
   "outputs": [],
   "source": [
    "X2=pd.concat([X2,gen2,ssc_b2,hsc_s2,hsc_b2,deg2,work2,spe2],axis=1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "id": "a8c65998",
   "metadata": {},
   "outputs": [],
   "source": [
    "X2_tr,X2_te,y2_tr,y2_te=train_test_split(X2,y2,test_size=0.25, random_state=2)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 69,
   "id": "14989c9c",
   "metadata": {},
   "outputs": [],
   "source": [
    "y2_te=y2_te.sort_index()\n",
    "X2_te=X2_te.sort_index()\n",
    "X2_tr=X2_tr.sort_index()\n",
    "y2_tr=y2_tr.sort_index()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 71,
   "id": "8242b381",
   "metadata": {},
   "outputs": [],
   "source": [
    "reg=LinearRegression()\n",
    "reg.fit(X2_tr,y2_tr)\n",
    "pred2=reg.predict(X2_te)\n",
    "pred3=reg.predict(X2_tr)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 72,
   "id": "be4a9702",
   "metadata": {},
   "outputs": [],
   "source": [
    "xp2=X2_te.index\n",
    "xp3=X2_tr.index"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 74,
   "id": "32cef765",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.legend.Legend at 0x2420d0e3dc0>"
      ]
     },
     "execution_count": 74,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.plot(xp2,y2_te,label='act')\n",
    "plt.plot(xp2,pred2,label='pred')\n",
    "plt.legend()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 75,
   "id": "947abefa",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.legend.Legend at 0x2420d14c760>"
      ]
     },
     "execution_count": 75,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.plot(xp3,y2_tr,label='act')\n",
    "plt.plot(xp3,pred3,label='pred')\n",
    "plt.legend()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "11d55904",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
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
   "version": "3.8.8"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
