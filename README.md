<img align="middle" src="./docs/design/OpenSelery-04.png" width="400"> 

> OpenSelery creates a dezentral framework for an free and transparent salary distribution. Software defined funding distribution creates a transparent and adaptable cash flow to your digital project and all its contributors, including your dependencies..

[![Actions Status](https://github.com/protontypes/openselery/workflows/openselery/badge.svg)](https://github.com/protontypes/openselery/actions)
![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/openselery/openselery)                               
[![Donate with bitcoin](https://en.cryptobadges.io/badge/small/3PVdiyLPR7MgaeFRJLW9mfuESZS2aAPX9w)](https://en.cryptobadges.io/donate/3PVdiyLPR7MgaeFRJLW9mfuESZS2aAPX9w)  

* [Project Slides](http://protontypes.eu/)
* [Roadmap](https://github.com/protontypes/openselery/wiki)
* [Knowledge-Links](https://github.com/protontypes/openselery/wiki/Knowledge-Links)

> Please keep in mind that OpenSelery is in an experimental state. The amount of funding should therefore be kept to a minimum. OpenSelery is not unit tested yet.

## Features

* Transparent Developers payout for your Github project.
* Dependencies scanning for most languages to include all developers.
* Only the sender needs an Coinbase account in the first instance.
* Investors create a transparent payout with public CI logs.

## How does OpenSelery work?

1. Configure OpenSelery based on the selery.yml file
2. Gather contributor information about the target project via the Github and Libraries.io API
3. Filter out contributors that have not made the email address visible in the Github profile.
4. Create a uniform weight distribution between all contributors. Custom distributions like release participation are under construction.
5. Randomly choose contributors based on the weights.
6. Payout Bitcoin to the chosen contributor email addresses via the Coinbase API. Contributor without a Coinbase account will get a email to claim the donation.

## Demo

[![demo](https://asciinema.org/a/qT8m8Tbvt2Fwck077FLGVjMn1.svg)](https://asciinema.org/a/qT8m8Tbvt2Fwck077FLGVjMn1?autoplay=1)

## Usage

### Create the access Tokens and Accounts

1. [Github](https://github.com/settings/tokens) and [Libraries.io](https://libraries.io/api) tokens are easy to obtain. Simulate payouts without the [coinbase token](https://www.coinbase.com/settings/api) to find the right settings. Configure the [access control settings](https://github.com/protontypes/openselery/wiki/Coinbase-Settings) of the automated coinbase account.

2. Install with [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/):

```bash
cd ~
git clone https://github.com/protontypes/openselery.git
cd openselery
./build.sh
```

3. Create a dedicated coinbase account with limited amounts. OpenSelery is based on the APIs of The Libraries.io, Github and Coinbase. To provide service you need create tokens in the corresponding accounts. Setting simulation to false will require your coinbase tokens.

4. Never transfer or store large values with automated cryptocurrency wallets. Use recurring automated transaction from another account to recharge you wallet on a regular base. 

5. Transfer some money to this wallet for testing. See the [price list](https://help.coinbase.com/en/coinbase/trading-and-funding/pricing-and-fees/fees.html) for transfering money into the coinbase account.

6. Create a read only token file for your user:

```bash
mkdir ~/.openselery/
touch ~/.openselery/tokens.env
chmod 400 ~/.openselery/tokens.env
```

7. Add your tokens API keys to the local file. **Never store this tokens in a repository**.

```bash
nano ~/.openselery/tokens.env

COINBASE_TOKEN=XXXXXXXXXXXXXXXX
COINBASE_SECRET=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
GITHUB_TOKEN=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
LIBRARIES_API_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
INITIATE_PAYOUT=True
```

8. Configure your distribution in the `selery.yml` file located in ~/.openselery/ or your <target_repository>
The `simulation` paramter is set `True` by default. You will need the `COINBASE_TOKEN` and `COINBASE_SECRET` when setting Simulation to False in your selery.yml. You can use the `selery.yml` template from the OpenSelery project, modifiy it and copy it into you <target_repository>. 


### Clone OpenSelery
      
```bash
git clone https://github.com/protontypes/openselery.git
```  

### Fund the target repository

This will send cryptocurrency to weighted random product contributors with a valid email address on the git platform or git commit:

#### Run as dockerized Command-Line Tool

```bash
env $(cat ~/.openselery/tokens.env) ~/openselery/run.sh <target_repository>
```

#### Run nativly on Debian / Ubuntu

1. Install Dependencies

```bash
sudo apt update && apt install git ruby ruby-dev ruby-bundler build-essentail curl python3-pip
cd openselery
bundle install 
pip install -r requirements.txt
```

2. Run OpenSelery

```bash
TARGET_DIR=<target_repository> && env $(cat ~/.openselery/tokens.env) python3 selery.py --config $TARGET_DIR/selery.yml --directory $TARGET_DIR --result results
```
    
### Continuous Integration  
Use the [github template](https://github.com/protontypes/seleryexample) to create a new selery in your project.


## Contribution
Constributors on the master branch will probably get emails with cryptocurrency. Only git profiles with emails on the Github profile page are considered. We will never send a URL in the note.

  *Artwork by Miriam Winter*
