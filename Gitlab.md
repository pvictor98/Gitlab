iamadmin=K341USSA
iampass=zS*SSy2c
iamhost=https://$(oc get route cp-console -o jsonpath="{.spec.host}")
iamaccesstoken=$(curl -sk -X POST -H "Content-Type: application/x-www-form-urlencoded;charset=UTF-8" -d "grant_type=password&username=$iamadmin&password=$iampass&scope=openid" $iamhost/idprovider/v1/auth/identitytoken | jq -r .access_token)

zenhost=
zentoken=$(curl -sk "$zenhost/v1/preauth/validateAuth" -H "username:$iamadmin" -H "iam-token: $iamaccesstoken" | jq -r .accessToken)


$ curl -vk https://cpd-bawdev.apps.cp4badevsaibocp.saibnet2.saib.com
* Rebuilt URL to: https://cpd-bawdev.apps.cp4badevsaibocp.saibnet2.saib.com/
* Uses proxy env variable no_proxy == '*.saibnet2.saib.com,*.saibsit.com,localhost,127.0.0.1,gitlab-dev.saibnet2.saib.com,172.16.231.241,nexus-dev.saibnet2.saib.com,10.10.24.240,*.icap.com.sa'
* Uses proxy env variable https_proxy == 'http://proxy.saib.com.sa:8080'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 172.31.1.57...
* TCP_NODELAY set
* Connected to proxy.saib.com.sa (172.31.1.57) port 8080 (#0)
* allocate connect buffer!
* Establish HTTP proxy tunnel to cpd-bawdev.apps.cp4badevsaibocp.saibnet2.saib.com:443
> CONNECT cpd-bawdev.apps.cp4badevsaibocp.saibnet2.saib.com:443 HTTP/1.1
> Host: cpd-bawdev.apps.cp4badevsaibocp.saibnet2.saib.com:443
> User-Agent: curl/7.61.1
> Proxy-Connection: Keep-Alive
> 
< HTTP/1.1 200 Connection established
< 
* Proxy replied 200 to CONNECT request
* CONNECT phase completed!
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
} [5 bytes data]
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
} [512 bytes data]
* CONNECT phase completed!
* CONNECT phase completed!
  0     0    0     0    0     0      0      0 --:--:--  0:00:57 --:--:--     0
  0     0    0     0    0     0      0      0 --:--:--  0:01:15 --:--:--     0
* Closing connection 0
curl: (35) OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to cpd-bawdev.apps.cp4badevsaibocp.saibnet2.saib.com:443 
Cleaning up project directory and file based variables

___________________________________________________

stages:
  - upload

before_script:
  - |
    for var in uatrouterca uatcacrt devrouterca devcacrt cpddev cpduat
    do
      eval "echo \"\$$var\"" > ${var}.crt
      cp ${var}.crt /etc/pki/ca-trust/source/anchors/

 upload-to-nexus:
  stage: upload
  script:
    # Create file
    - touch testfile.txt
    - echo "created from gitlab pipeline $(date)" > testfile.txt

    # Upload to Nexus
    - |
      curl -v \
      -u "$NEXUS_USER:$NEXUS_PASSWORD" \
      --upload-file testfile.txt \
      "https://nexus.test.com/repository/my-raw-repo/gitlab/testfile.txt"


Checking out 62562a09 as detached HEAD (ref is main)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
00:00
$ unset cd
$ for var in uatrouterca uatcacrt devrouterca devcacrt cpddev cpduat # collapsed multi-line command
bash: eval: line 192: syntax error: unexpected end of file
Cleaning up project directory and file based variables
00:00
ERROR: Job failed: exit status 2
