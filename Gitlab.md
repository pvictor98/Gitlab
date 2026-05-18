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
  - mkdir -p /etc/pki/ca-trust/source/anchors/

  - |
    for cert in \
      "$uatrouterca" \
      "$uatcacrt" \
      "$devrouterca" \
      "$devcacrt" \
      "$cpddev" \
      "$cpduat"
    do
      if [ -n "$cert" ] && [ -f "$cert" ]; then
        echo "Adding $cert"
        cp "$cert" /etc/pki/ca-trust/source/anchors/
      else
        echo "Skipping missing cert: $cert"
      fi
    done

  - update-ca-trust

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

_____

Fetching changes with git depth set to 20...
Reinitialized existing Git repository in /opt/gitlab-runner/builds/RzjU9CuY/0/cp4ba/cp4ba-pipeline/.git/
Checking out 6567c576 as detached HEAD (ref is main)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
00:01
$ mkdir -p /etc/pki/ca-trust/source/anchors/
$ for cert in \ # collapsed multi-line command
Skipping missing cert: -----BEGIN CERTIFICATE-----
MIIDDDCCAfSgAwIBAgIBATANBgkqhkiG9w0BAQsFADAmMSQwIgYDVQQDDBtpbmdy
ZXNzLW9wZXJhdG9yQDE3NjM2NDgwMzEwHhcNMjUxMTIwMTQxMzUwWhcNMjcxMTIw
MTQxMzUxWjAmMSQwIgYDVQQDDBtpbmdyZXNzLW9wZXJhdG9yQDE3NjM2NDgwMzEw
ggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCsZ200rjXVuJCxZgQURiJ6
dE6+tfZcsg1K6VUrVqCU/LC3WmUzxYU13EUYJ9839Nw9x8ClYaJLDzEHxvggAxmm
FOt99YmmvYFvtDoQpiRN0qMz04ugmJA7saAn03DKE83yzkbGhG3ygDAHx5+pnyjJ
A2c5EIAgGlW81U/u7DUQDSVFZUClrMxChS6YLA6EkNy8ar7O8diGDNdgCrypz3ea
WtnKDTLbiUvZ5Dp5loH87SZJdCPKhcE7MCX7miMw1HWzEM8yozbNSv2Q9H5o9qRi
tXY/nhwSnIxFu6OoFkS7X0pcc0ebtecrV/fdI5iIQ3meQyt2cRpHeVBu1+TMDno/
AgMBAAGjRTBDMA4GA1UdDwEB/wQEAwICpDASBgNVHRMBAf8ECDAGAQH/AgEAMB0G
A1UdDgQWBBTE4fJ5rFxPBE/kkUGwDa+fOqmaBjANBgkqhkiG9w0BAQsFAAOCAQEA
MLz5rlJU/qHj6ZNlB9cvaxR8L8Ery/pCzKx0DHJiYDPKxk2POoTphkyYO/3N6Bij
TM+e7WVwo9R8Ew4Jo2UkZElXXkrr1v2kzeAUIbVZ/9j2QOir8GPfT/Q+WzqIIqHF
vD2B6CMkzF6iSAakxGL8gaJs1gebEtxFeF1NuCNb8Aw8bqqBIE4RisCuhgUOfajT
UiAMW+Ucw3YXeeN51zQKtsjkbj3zMtPsjWfQJlyQAQpO9kb/nN4w0pFNup3QWlNv
+HA2scrPPSXwyzK9Ymd5hfyQq76x2c1hCcVWBkAaqpMCCIABkF0BIRTnZDqxhXJQ
1KWUoiEgulOpygpMQDKLTQ==
-----END CERTIFICATE-----
Skipping missing cert: -----BEGIN CERTIFICATE-----
.....
....
$ update-ca-trust
p11-kit: couldn't create file: /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt: Unknown error 13
p11-kit: couldn't create file: /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem: Unknown error 13
p11-kit: couldn't create file: /etc/pki/ca-trust/extracted/pem/email-ca-bundle.pem: Unknown error 13
p11-kit: couldn't create file: /etc/pki/ca-trust/extracted/pem/objsign-ca-bundle.pem: Unknown error 13
p11-kit: couldn't create file: /etc/pki/ca-trust/extracted/java/cacerts: Unknown error 13
p11-kit: couldn't create file: /etc/pki/ca-trust/extracted/edk2/cacerts.bin: Unknown error 13
Cleaning up project directory and file based variables
00:00
ERROR: Job failed: exit status 1




before_script:
  # Use sudo to create the directory if it doesn't exist
  - sudo mkdir -p /etc/pki/ca-trust/source/anchors/

  - |
    for cert in \
      "$uatrouterca" \
      "$uatcacrt" \
      "$devrouterca" \
      "$devcacrt" \
      "$cpddev" \
      "$cpduat"
    do
      # Now that these are "File" type variables, $cert will contain a file path
      if [ -n "$cert" ] && [ -f "$cert" ]; then
        echo "Adding certificate from path: $cert"
        sudo cp "$cert" /etc/pki/ca-trust/source/anchors/
      else
        echo "Skipping missing or invalid cert path: $cert"
      fi
    done

  # Use sudo to update the system trust store
  - sudo update-ca-trust
