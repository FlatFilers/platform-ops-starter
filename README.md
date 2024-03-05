# platform-ops-starter
This repo is a starter project used by the Flatfile Platform Operations team. 

## Getting Started
1. Go to one of the Flatfile Platform environments and create an account
1. Switch to the Development environment, go to `Developer Settings` and copy your secret key
1. Deploy or run the listener locally -- `pnpm run dev --api-url=https://platform.uk.flatfile.com/api` (you only need the `--api-url` if you are using a region outside of the default US)
1. Create a new space in the Flatfile dashboard
1. Upload a file


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