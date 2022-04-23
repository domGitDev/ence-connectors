# ##########################################################

    LOW LATENCY EXCHANGE CONNECTOR

    AUTHOR: DOM SILVA

    COPYRIGHT: STELGIC.

    WEBSITE: https://www.stelgic.com

    Developed and tested with Centos 7/8

# ##########################################################

# DESCRIPTION

Low latency and unified cryptocurrency market data connector developed in c++17 for Linux.
Market data and order update are done via websocket if available. To ensure scalability and data loss prevention it has been developed on top of kafka stream. 

# SUPPORTED EXCHANGES (futures trading) 

- Binance
- OKX

# INSTALL APACHE KAFKA

https://www.digitalocean.com/community/tutorials/how-to-install-apache-kafka-on-centos-7


# BUILD DEPENDENCIES

``` bash
cd scripts
sudo ./install_libraries.sh
```

### ADD TO ~/.bashrc

``` bash
export LD_LIBRARY_PATH=/usr/local/openssl/lib:/usr/local/cppkafka/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/cpr/lib:/usr/local/curl/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/boost/lib:/usr/local/librdkafka/lib64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/jsoncpp/lib64:/usr/local/libwebsockets/lib:$LD_LIBRARY_PATH
```

### GET Connections

``` bash
git clone https://github.com/domGitDev/ence-connectors.git 
cd ence-connectors
unzip ence-connectors.zip
ln -s libs-nodb libs
```

### ADD EXCHANGES API KEYS

- for each exchange you like to use:
    - edit [EXCHANGE_NAME].txt and replace words that starts with YOUR_API_KEY and  YOUR_API_SECRET


### HOW TO RUN

```
./connector -t bncfutbook -d obook -e binance -f 1 > bncobook.log &
./connector -t bncfutprice -d price -e binance -f 1 > bncprices.log &
./connector -t bncfut -d orders -e binance -v 1 -f 1 > bncexecution.log &
```

# SUBSCRIBE KAFKA CONSUMER FOR UPDATES VIA TOPICS

- BINANCE TOPICS: bncfutbook; bncfutprice; bncupdates
- OKX TOPICS: okxbook; okxprice; okxupdates


### EXAMPLE

``` bash
cd ence-connectors/scrips
pip install kafka-python
```
- consume price data
```
python consumer.py -b 127.0.0.1:9092 -t bncprice
```

- consume order book data
```
python consumer.py -b 127.0.0.1:9092 -t bncobook
```

- Price output example
```
[Symbol, Date, time, second, millisecond, price, quantity, side, timestamp]
```
```
['BTCUSDT', '20220423', '1830', 23, 224, 39775.1, 0.008, -1, 1650738623224]
['BTCUSDT', '20220423', '1830', 23, 296, 39775.0, 0.057, 1, 1650738623296]
['BTCUSDT', '20220423', '1830', 23, 296, 39775.0, 0.009, 1, 1650738623296]
```

- order book output example
```
[Symbol, timestamp, updateId, snapshot/changes, bid=[[price, quantity]], ask=[[price, quantity]]]
```

```
['BTCUSDT', 1650738956852, 1421997336645, 'change', [[39728.3, 2.197], [39728.1, 0.001], [39728.0, 0.017], [39727.5, 0.001], [39727.2, 4.034]], [[39728.4, 0.988], [39728.9, 0.699], [39729.0, 0.011], [39729.4, 0.003], [39729.5, 0.003]]]
```

