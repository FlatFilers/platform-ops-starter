# platform-ops-starter
This repo contains a few starter projects used by the Flatfile Platform Operations team to ensure proof-of-life on environments. 

## Requirements
- Node 18+
- PNPM v7

```bash
# clone this repo:
git clone https://github.com/FlatFilers/platform-ops-starter.git
cd platform-ops-starter/use-cases/listener

# ensure you are using at least NodeJS v18, e.g. if you use NVM:
nvm use 18

# install dependencies:
pnpm i
```


# Getting Started with the Listener
Flatfile uses the concept of an "agent" and an event system to create and manage your Spaces and Workbooks (e.g. the things that hold data) as well as manipulate data. An easy way to get started is to run the agent locally on your machine where we will send all the events to be handled. Get started quickly by:
1. Go to one of the Flatfile Platform environments and create an account, then install the Portal application
1. Switch to the Development environment, go to `Developer Settings` and copy your secret key
2. If you are running the `develop` command (not deploy) you will also need to set the environment variable `export FLATFILE_API_URL=https://platform.uk.flatfile.com/api` (or whichever environment you are using)
1. Deploy or run the listener locally -- `pnpm run dev --api-url=https://platform.uk.flatfile.com/api` (you only need the `--api-url` if you are using a region outside of the default US)
1. Choose the API key option and paste in your secret key 
1. Create a new space in the Flatfile dashboard
1. Upload a file

## Deploying 
After getting the basics sorted locally, deploy your agent to the Flatfile cloud so that it can listen for events independent of your local machine:
1. `Ctrl+c` to stop running the local agent
1. `pnpm run deploy --api-url=...`
1. Choose the API key option and paste in your secret key 
1. Now, go to the Flatfile dashboard and create a space - this will open a new tab where you can upload data

## Data Shape
It's a pretty simple list of contacts: 
```
First Name   Last Name   Email
Esther      Swanson     nafusve@eti.co
Myrtle      Curtis      wiceumu@abtokur.la
Isabel      Goodman     suepi@sa.org
Rosie       Douglas     ec@zo.ss
Francis     Dunn        ura@zikgez.as
Nina        Torres      tutfe@vopbe.lu
Gabriel     Gilbert     nuvap@danogfu.tz
Maude       McCarthy    sadmojjeb@babse.om
Mabelle     Brooks      hi@jibum.tw
Elnora      Ballard     ijvic@woznuppam.pg
```

Generate your own or use the provided fake data `getting-started.csv`

# Getting Started with React
The `use-cases/react` project contains a minimal starter for embedding a space into a webpage. A new space is created each time it launches. To use this project, go to your dashboard Developer Settings, and collect the following:
- Environment ID
- Publishable key

Edit the file `use-cases/react/src/App.tsx` and plug in the environment ID and the publishable key, then add the appropriate regional API endpoint. Change directories into the `react` folder and run `npm run start`

# Resources

## Egressing Data
The `sheets/:sheetId/records` endpoint is used to page through data. To obtain the ID for the sheet, first copy your environment ID and secret key from the `Developer Settings` page, fetch a list of Spaces to find the Space ID, then fetch the workbook ID (alternatively, reverse engineer the ID from the network tab while in the space itself). 

An example via cURL:
```bash
# Fetch 
curl -G https://platform.uk.flatfile.com/api/v1/spaces \
     -H "Authorization: Bearer sk_ABC" \
     -d environmentId=uk_env_123

# Use the Space ID from previous request:
curl -G https://platform.uk.flatfile.com/api/v1/workbooks \
     -H "Authorization: Bearer sk_ABC" \
     -d spaceId=uk_sp_123 \
     -d includeCounts=true

# Use the Sheet ID from the previous request:
curl https://platform.uk.flatfile.com/api/v1/sheets/uk_sh_123/records \
     -H "Authorization: Bearer sk_ABC"
```

## Regions
| Region | App URL | API URL |
| ------ | ------- | ------- |
| US (default) | https://platform.flatfile.com | https://platform.flatfile.com/api |
| CA     | https://platform.ca.flatfile.com | https://platform.ca.flatfile.com/api |
| EU     | https://platform.eu.flatfile.com | https://platform.eu.flatfile.com/api |
| UK     | https://platform.uk.flatfile.com | https://platform.uk.flatfile.com/api |
| AU     | https://platform.au.flatfile.com | https://platform.au.flatfile.com/api |

## Resources
- [Authenticating](https://flatfile.com/docs/developer-tools/security/authentication)
- [Fetching Records](https://reference.flatfile.com/api-reference/records/get)
- [Generate Fake Data](https://www.mockaroo.com/)
