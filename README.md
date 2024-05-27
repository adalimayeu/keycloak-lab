# Keycloak demo

## Usage

```sh
python3 -m venv .venv
source .venv/bin/activate
export PATH="$(pwd)/.venv/bin:$PATH"
pip3 install ansible requests

docker-compose up -d
ansible-playbook config/provision.yml -v

# + manual sync ldap groups to Keycloak
```

## Grafana integration

1. get user's token from Keycloak
```sh
curl \
 -d "grant_type=password" \
 -d "scope=openid" \
 -d "client_id=grafana" \
 -d "client_secret=grafana-secret" \
 -d "username=jsmith" \
 -d "password=jsmith" \
 http://127.0.0.1:8080/realms/labrealm/protocol/openid-connect/token | jq -r .access_token
```

1. decode JWT - https://jwt.io/

```json

    ## attached realm roles will be shown there
    "realm_access": {
        "roles": [
            "offline_access",
            "admin",
            "uma_authorization",
            "default-roles-labrealm"
        ]
    },
    ## attached client roles will be shown there
    "resource_access": {
        "grafana": {
            "roles": [
                "grafanaAdmin"
            ]
        },
...
```


## LDAP user federation

step-by-step page - https://www.olvid.io/keycloak/ldap-federation/



