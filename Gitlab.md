iamadmin=K341USSA
iampass=zS*SSy2c
iamhost=https://$(oc get route cp-console -o jsonpath="{.spec.host}")
iamaccesstoken=$(curl -sk -X POST -H "Content-Type: application/x-www-form-urlencoded;charset=UTF-8" -d "grant_type=password&username=$iamadmin&password=$iampass&scope=openid" $iamhost/idprovider/v1/auth/identitytoken | jq -r .access_token)

zenhost=
zentoken=$(curl -sk "$zenhost/v1/preauth/validateAuth" -H "username:$iamadmin" -H "iam-token: $iamaccesstoken" | jq -r .accessToken)
