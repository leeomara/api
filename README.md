# Proof's REST/HTTP/JSON API documentation

## Requirements

Readers of this document will need to have

- an account at an instance of the Proof platform (e.g. app.proofgov.com)
- a self-serve API token generated for that account.

### API Token Generation

1. Log into the app.
2. Go to your profile page and enabled the Developer Tools option.
3. Click on the Self-Serve API Token link under Developer Tools.

To get a new a token, delete the old one and then go back to the self-serve link.

1. Get the ID of the token you want to delete from the `/self-serve/api-token` endpoint.
2. Perform a DELETE request with that ID to `/self-serve/api-tokens/<id>`
   e.g.

```bash
API_TOKEN=6f7ad9f4fc06b73c2105cf1f3b5c287b02ff74a9c83b1a887cfbad04afa0f746
API_TOKEN_ID_TO_DELETE=3

curl \
  -X DELETE \
  "http://app.proofgov.com/self-serve/api-tokens/${API_TOKEN_ID_TO_DELETE}" \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${API_TOKEN}"
```

3. Repeate the initial self-serve token generation steps.

## Example Applications

### Concerning Form Submissions

- [A basic example, downsampling implements some custom access control](https://github.com/proofgov/example-form-query-api)
- [A rate-limiting application](https://github.com/proofgov/example-app-capacity-management)

## Notes

1. In this documentation, `${API_TOKEN}` is meant to refer to the token provided by Proof for your integration.
2. We use `${API_HOST}` to refer to the hostname of the Proof instance you're meaning to interact with. For most users, this will be app.proofgov.com. For users of special single-tenant installs, this should the hostname you would visit in your browser (e.g. trac.ynet.gov.yk.ca).

In both cases, we're writing these as interpolated shell variables to facilitate copying and pasting snippets directly into a shell session.

## Endpoints

- [Users](users-endpoint.md)
- [Routings](routings-endpoint.md)
- [Forms](forms-endpoints.md)

Return to Office
- [Builidings](Return%20to%20Office/Buildings%20Endpoints.md)
- [BuildingAccessAppointments](Return%20to%20Office/Building%20Access%20Appointments.md)

### Common requirements

All requests must contain headers:

- `Authorization: Bearer {PROOF PROVIDED API TOKEN}`
- `Accept: application/json`
- `Content-Type: application/json`

### Paging

Endpoints that return multiple records will do so in pages.

`page` and `per_page` are the control parameters.

No more than 1k records will be returned on any request;
larger values of `per_page` will be ignored

#### Formatting

##### Dates

All dates sent to and returned from the system will be formatted as `YYYY-MM-DD`.
