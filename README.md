- 👋 Hi, I’m @Jbones4life36
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Jbones4life36/Jbones4life36 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#!/usr/bin/env pwsh

$app_id = YOUR_APP_ID
$private_key_path = "YOUR_PATH_TO_PEM"

$header = [Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes((ConvertTo-Json -InputObject @{
  alg = "RS256"
  typ = "JWT"
}))).TrimEnd('=').Replace('+', '-').Replace('/', '_');

$payload = [Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes((ConvertTo-Json -InputObject @{
  iat = [System.DateTimeOffset]::UtcNow.AddSeconds(-10).ToUnixTimeSeconds()
  exp = [System.DateTimeOffset]::UtcNow.AddMinutes(10).ToUnixTimeSeconds()
  iss = $app_id
}))).TrimEnd('=').Replace('+', '-').Replace('/', '_');

$rsa = [System.Security.Cryptography.RSA]::Create()
$rsa.ImportFromPem((Get-Content $private_key_path -Raw))

$signature = [Convert]::ToBase64String($rsa.SignData([System.Text.Encoding]::UTF8.GetBytes("$header.$payload"), [System.Security.Cryptography.HashAlgorithmName]::SHA256, [System.Security.Cryptography.RSASignaturePadding]::Pkcs1)).TrimEnd('=').Replace('+', '-').Replace('/', '_')
$jwt = "$header.$payload.$signature"
Write-Host $jwt
#!/usr/bin/env bash

set -o pipefail

app_id=$1 # App ID as first argument
pem=$( cat $2 ) # file path of the private key as second argument

now=$(date +%s)
iat=$((${now} - 60)) # Issues 60 seconds in the past
exp=$((${now} + 600)) # Expires 10 minutes in the future

b64enc() { openssl base64 | tr -d '=' | tr '/+' '_-' | tr -d '\n'; }

header_json='{
    "typ":"JWT",
    "alg":"RS256"
}'
# Header encode
header=$( echo -n "${header_json}" | b64enc )

payload_json='{
    "iat":'"${iat}"',
    "exp":'"${exp}"',
    "iss":'"${app_id}"'
}'
# Payload encode
payload=$( echo -n "${payload_json}" | b64enc )

# Signature
header_payload="${header}"."${payload}"
signature=$(
    openssl dgst -sha256 -sign <(echo -n "${pem}") \
    <(echo -n "${header_payload}") | b64enc
)

# Create JWT
JWT="${header_payload}"."${signature}"
printf '%s\n' "JWT: $JWT"
6010#!/usr/bin/env python3
from jwt import JWT, jwk_from_pem
import time
import sys

# Get PEM file path
if len(sys.argv) > 1:
    pem = sys.argv[1]
else:
    pem = input("Enter path of private PEM file: ")

# Get the App ID
if len(sys.argv) > 2:
    app_id = sys.argv[2]
else:
    app_id = input("Enter your APP ID: ")

# Open PEM
with open(pem, 'rb') as pem_file:
    signing_key = jwk_from_pem(pem_file.read())

payload = {
    # Issued at time
    'iat': int(time.time()),
    # JWT expiration time (10 minutes maximum)
    'exp': int(time.time()) + 600,
    # GitHub App's identifier
    'iss': app_id
}

# Create JWT
jwt_instance = JWT()
encoded_jwt = jwt_instance.encode(payload, signing_key, alg='RS256')

print(f"JWT:  {encoded_jwt}")
67a7811ec75b6b513a58c7b93f99cef8ab639a02
     {
    "code": "0",
    "msg": "",
    "data": [
        {
            "collectionName": "BoredApeYachtClub",
            "tokenContractAddress": "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d",
            "totalSupply": "10000",
            "holder": "5537",
            "avgPrice": "39335.9037",
            "transactionNumber": "156",
            "activeUser": "67",
            "transactionVolume": "1927459.2813000001",
            "transactionVolumeUsd": "2992611829.332006",
            "floorPrice": "24.38995001",
            "lastPrice": "26.9",
            "collectionLogoUrl": "https://static.coinall.ltd/cdn/nft/files/collection/205-logo.webp"
        }
    ]
}
1927459.28132992611829https://static.coinall.ltd/cdn/nft/files/collection/205-logo.webp{
    "code": "0",
    "msg": "",
    "data": [
        {
            "totalMarketCap": "25854298568.561043",
            "totalHolder": "14315586",
            "dailyTradingVolume": "8816152.74027341",
            "dailyTransaction": "30656"
        }
    ]
}
25854298568GET /api/v5/explorer/nft/nft-stats-overviewTap on a clip to paste it in the text box.{
    "code": "0",
    "msg": "",
    "data": [
        {
            "chainFullName": "Ethereum",
            "chainShortName": "ETH"
        },
        {
            "chainFullName": "Polygon",
            "chainShortName": "POLYGON"
        },
        {
            "chainFullName": "Optimism",
            "chainShortName": "OPTIMISM"
        },
        {
            "chainFullName": "OKT Chain",
            "chainShortName": "OKTC"
        },
        {
            "chainFullName": "Avalanche-C",
            "chainShortName": "AVAXC"
        },
        {
            "chainFullName": "ARBITRUM",
            "chainShortName": "ARBITRUM"
        },
        {
            "chainFullName": "BNB Chain",
            "chainShortName": "BSC"
        }
    ]
}
GET /api/v5/explorer/nft/nft-stats-overview8816152.74022585429856814315586
