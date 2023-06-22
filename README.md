# MAThesis


<div align="center">
</div>
Thesis: A flicker in the dark. Exploring MEV in the Ethereum Ecosystem
This project provides a collection of tools to measure and analyze frontrunning attacks on private pools such as Flashbots. You can find the full Thesis [here](add github and set to private, just for Mr Ittner & Oettler) and all my collected data is available for [download](MY GOOGLE DRIVE Will Provide ALL DATA, upgraded to 100gb/year).

Quick Start
A Docker container with all the dependencies required can be found [here](My Google Drive Link image: readyfetch).

Follow the instructions below to set it up:

Note: Please check your system's capabilities and adjust accordingly.

shell
Copy code
docker pull sebpet1337/0x:readyfetch
docker run --name naughtykai -m 16g --memory-swap="24g" -p 8888:8888 -it readyfetch
You can then start an instance of MongoDB inside the container:

shell
Copy code
mkdir -p /data/db && mongod --fork --logpath /var/log/mongod.log
To run the MEV measurement scripts, use the following commands inside the container:

Note: A Quicknode RPC connection to a fully synched Ethereum archive node is provided. You can change this to your own (but be aware of the limits!) in the PROVIDER variable in data-collection/mev/utils/settings.py.

shell
Copy code
# Run the sandwich measurement script
cd /root/data-collection/mev/sandwiches
python3 sandwiches.py <BLOCK_RANGE_START>:<BLOCK_RANGE_END> # For example: python3 sandwiches.py 15900000:15901000

# Run the arbitrage measurement script
cd /root/data-collection/mev/arbitrage
python3 arbitrage.py <BLOCK_RANGE_START>:<BLOCK_RANGE_END> # For example: python3 arbitrage.py 11706655:11706655

# Run the liquidation measurement script
cd /root/data-collection/mev/liquidation
python3 liquidation.py <BLOCK_RANGE_START>:<BLOCK_RANGE_END> # For example: python3 liquidation.py 11181773:11181773

# To download new price data from the Coingecko API and save to prices.json
# This is already done and up to date till March 8th 2023 
# If you want to update prices, change the settings. If you manually imported prices.json, this is not necessary.
cd /root/data-collection/mev/utils
# Modify the settings.py
# Set Update_Prices = True 
You can download and import the Flashbots blocks into MongoDB by running the following commands inside the container. Note that this step is already completed in the 'readyfetch' image.

shell
Copy code
cd /root/data-collection/flashbots
python3 import_flashbots_data.py
To run the analysis, launch the Jupyter notebook server inside the container using the following commands and then open http://localhost:8888 on your browser:

shell
Copy code
cd /root/analysis
jupyter notebook --port=8888 --no-browser --ip=0.0.0.0 --allow-root --NotebookApp.token='' --NotebookApp.password=''
Useful Docker Commands
Check if you're up and running
docker ps Access MongoDB via terminal
docker exec -it naughtykai mongo

Useful MongoDB Commands
shell
Copy code
show databases
use flashbots
show collections
db.collectionName.find() 
Attribution
This project is a slightly modified version of the following paper:

bibtex
Copy code
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
