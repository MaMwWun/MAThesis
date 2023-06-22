# Thesis: A flicker in the dark. Exploring MEV in the Ethereum Ecosystem </h1>


A collection of tools to measure and analyze frontrunning attacks on private pools such as [Flashbots](https://docs.flashbots.net). 
My Thesis can be found [here](add github and set to private, and my data is available for download [here](MY GOOGLE DRIVE Will Provide ALL DATA.

SAME LAYOUT AS THESIS
**Code**

This repository contains:

1. A script adapted from Torres, with adjustments to the sleep parameter at Coingecko prices and fixes for Python utility errors.
2. Code for scraping the ZeroMev API and visualizing the data using Python and Pandas. This script generates monthly MEV type absolute values, USD values, and detects outlier blocks.
3. A simple script for determining the start and end of a month, and the corresponding block range.

**Datasets**

The datasets included in this repository are:

1. Data from Flashbots in Pan mev-inspect Script for Blocks 14,444,725 - 16,700,000
   - `sandwich_results.json`
   - `arbitrage_results.json`
   - `liquidation_results.json`
2. Data from ZeroMev scraped for Blocks 14,444,725 - 16,700,000
   - `ZeroMevBlock14444725:16.700.000`
3. A table detailing the months and dates corresponding to the ZeroMev data.

**API Link:**
[Insert API Link Here flashbotsblocksandzeromev]

The comprehensive datasets and code made available in this appendix are intended to foster further research and understanding of MEV in the Ethereum ecosystem.



## Quick Start to run Weintraub et al. Script 

A container with all the dependencies can be found [here] (My Google Drive Link image: readyfetch)

To run the container, please install docker and run:
**pls check your systems capabilities and adjust accordingly**

``` shell
docker pull sebpet1337/0x:readyfetch
docker run --name naughtykai -m 16g --memory-swap="24g" -p 8888:8888 -it readyfetch

```

Afterwards, start an instance of MongoDB inside the container:

``` shell
mkdir -p /data/db && mongod --fork --logpath /var/log/mongod.log
```


To run the MEV measurement scripts, simply run inside the container the following commands:

**Quicknode RPC connection to a fully synched Ethereum archive node is provided - can change to own (watch limits!) in  ```PROVIDER``` in ```data-collection/mev/utils/settings.py``` !!**

``` shell
# Run the sandwich measurement script
cd /root/data-collection/mev/sandwiches
python3 sandwiches.py <BLOCK_RANGE_START>:<BLOCK_RANGE_END> # For exmaple: python3 sandwiches.py 15900000:15901000

# Run the arbitrage measurement script
cd /root/data-collection/mev/arbitrage
python3 arbitrage.py <BLOCK_RANGE_START>:<BLOCK_RANGE_END> # For exmaple: python3 arbitrage.py 11706655:11706655

# Run the liquidation measurement script
cd /root/data-collection/mev/liquidation
python3 liquidation.py <BLOCK_RANGE_START>:<BLOCK_RANGE_END> # For exmaple: python3 liquidation.py 11181773:11181773


Download new price data from Coingecko Api and save to prices.json
-> Already done and up to date till March 8th 2023 
# change if you want to update prices, however if manually imprted prices.json no need
cd /root/data-collection/mev/utils
change Settings.py 
set Update_Prices = True 

```

**Download & Import the flashbots blocks into MongoDB by running inside the container the following commands:**
-> This is already provided in image: readyfetch

``` shell
cd /root/data-collection/flashbots
python3 import_flashbots_data.py
```


**To run the analysis, please launch the Jupyter notebook server inside the container using the following commands and then open up http://localhost:8888 on your browser:**

``` shell
cd /root/analysis
jupyter notebook --port=8888 --no-browser --ip=0.0.0.0 --allow-root --NotebookApp.token='' --NotebookApp.password=''
```

## Useful Docker Commands
check if youre up and running
```docker ps ```
see MongoDB in Terminal
```docker exec -it naughtykai mongo ```

## Useful Commands MongoDB

```
show databases
use flashbots
show collections
db.collectioName.find() 
```





## Attribution
This is a slightly mofified version of this paper:

``` bibtex
@inproceedings{
  aflashbotinthepan, 
  address={Nice, France}, 
  title={A Flash(bot) in the Pan: Measuring Maximal Extractable Value in Private Pools}, 
  ISBN={978-1-4503-9259-4}, 
  DOI={10.1145/3517745.3561448}, 
  booktitle={Proceedings of the 22nd ACM Internet Measurement Conference (IMC ’22)}, 
  publisher={Association for Computing Machinery}, 
  author={Weintraub, Ben and Ferreira Torres, Christof and Nita-Rotaru, Cristina and State, Radu}, 
  year={2022} 
}
```
