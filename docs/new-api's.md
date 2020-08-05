<!-- theme: success -->

> ### Mission Accomplished!
>
> Here is my success callout!

<!-- title: My Table Title -->

| Tables        |      Are      |   Cool |
| ------------- | :-----------: | -----: |
| ramesh   | right-aligned | \$1600 |
| col 2 is      |   centered    |   \$12 |
| zebra stripes |   are neat    |    \$1 |




Trust the server certificate
Get the certificates detail as shown in this example:

curl --request GET \
  --url 'https://<your-ppdm-server>:8443/api/v2/certificates?host=<host>&port=<port>&type=Host' \
  --header 'authorization: Bearer <access-token>'
The <host> is the vCenter FQDN or IP address. The <port> is the port number, usually 3009.

The response is similar to this example:

OK (200)

{
    "id": "MTAuMTEwLjQuMzk6MzAwOTpIT1NU",
    "host": "<host>",
    "port": "<port>",
    "notValidBefore": "Tue Apr 24 02:26:22 CST 2019",
    "notValidAfter": "Sat Apr 23 09:26:22 CST 2023",
    "fingerprint": "70832CF8AF7B957BE9EC25EB611804B0B80203E3",
    "subjectName": "CN=ddve, ...",
    "issuerName": "CN=ddve, ...",
    "state": "UNKNOWN",
    "type": "HOST"
}
Update the "state": "UNKNOWN" to "state": "ACCEPTED".

Trust the certificate as shown here:

curl --request PUT \
  --url https://<your-ppdm-server>:8443/api/v2/certificates \
  --header 'content-type: application/json' \
  --header 'authorization: Bearer <access-token>' \
  --data '<the-json-response-from-previous-request-with-state-set-to-ACCEPTED>'
You receive a response similar to this example:

OK (200)

{
    "id": "MTAuMTEwLjQuMzk6MzAwOTpIT1NU",
    ...
    "state": "ACCEPTED",
    ...
}
Create credentials for vCenter
In this case, ensure that you specify the credentials type as VCENTER.

The request body looks like this:

{"type": "VCENTER", "name": "<display-name>", "username": "<usr>", "password": "<pwd>" }
Add the vCenter:
The request would be similar to this one:

curl --request POST \
  --url https://<your-ppdm-server>:8443/api/v2/inventory-sources \
  --header 'content-type: application/json' \
  --header 'authorization: Bearer <access-token>' \
  --data '{
    "name": "<display-name>",
    "type": "VCENTER",
    "address": "<FQDN-or-IP-Address>",
    "port": "<port>",
    "credentials": {
        "id": "<credentials-id-refer-how-to-add-a-data-domain>"
    }
}'
The response would be like this response:

Created (201)

{
  "id": "e0f4a598-2065-4f27-aaad-45a475af71e6",
  ...
}