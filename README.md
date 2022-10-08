# events-service

Making API requests to Events Service for Tanzu Mission Control.

## Making requests to Event Stream API in Linux
Users first need to generate a [CSP Refresh Token](https://docs.vmware.com/en/VMware-Cloud-services/services/Using-VMware-Cloud-Services/GUID-E2A3B1C1-E9AD-4B00-A6B6-88D31FCDDF7C.html). This token can then be used to get a CSP Authorization Token.

Once the user has an authorization token they can then provide this as a bearer token in the `Authorization` header.

Using `curl` in Bash/ZSH for example we could run the following:

```
$ export CSP_TOKEN="VALUE_FROM_API_TOKENS_PAGE"

$ export ACCESS_TOKEN=$(curl -s -X POST "https://console.cloud.vmware.com/csp/gateway/am/api/auth/api-tokens/authorize" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "refresh_token=$CSP_TOKEN" | jq -r '.access_token')

$ curl -s -H "Authorization: Bearer $ACCESS_TOKEN" \
'https://<org_name>.tmc.cloud.vmware.com/v1alpha1/events/stream'
```

## Making requests to Event Stream API using Python script:

A python script has been included for you to easily make calls to the events service api and stream events directly into your terminal. The script is named `event_stream_service_call.py`.

Usage of event_stream_service_call.py:

1. Make sure to install the requests python library required by this script by running the following in your terminal:

```
pip3 install requests --user
```

2. There are two arguments needed from the command line input in order to make a call to the event service stream api: `--csp_token` and `--tmc_url`

The `--csp_token` is the token which you can generate via CSP console with this link: https://console.cloud.vmware.com/csp/gateway/portal/#/user/tokens

The `--tmc_url` is the url to access your Tanzu Mission Control portal, which looks like: https://cna.tmc.cloud.vmware.com

Feed these two parameters via the command line in your terminal when running the script. For example:

```
python3 event_stream_service_call.py --csp_token=<VALUE_FROM_API_TOKENS_PAGE> --tmc_url='https://cna.tmc.cloud.vmware.com'
```

3. Trigger some events to your clusters via Tanzu Mission Control, and you'll find the events being streamed into your terminal as they happen!

4. In order to stop streaming the events and terminate the script, press control+c or control+z.
