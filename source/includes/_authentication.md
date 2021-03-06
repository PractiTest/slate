# Authentication

## Authentication Options
```shell
# Basic authentication:
    curl -H "Content-Type: application/json" \
     -u YOUR_EMAIL:YOUR_TOKEN  \
    https://api.practitest.com/api/v2/projects.json
# IS THE SAME AS (as parameters):
    curl -H "Content-Type: application/json" \
    https://api.practitest.com/api/v2/projects.json?developer_email=YOUR_EMAIL&api_token=YOUR_TOKEN
# IS THE SAME AS (via header):
    curl -H "Content-Type: application/json" \
    -H "PTToken: YOUR_TOKEN" \
    https://api.practitest.com/api/v2/projects.json
```
> Make sure to replace `YOUR_TOKEN` with your API token.

PractiTest uses API tokens for authentication. You can create a new API token by going to the Account Settings - "API Tokens", or going to your personal settings for Personal Tokens. Please visit <a href="https://www.practitest.com/help/account/account-api-tokens/" target="_blank">API tokens</a> for more information.
API expects the API-token to be included in all API requests to the server.
There are three ways to authenticate to PractiTest API V2 (you can choose anyone of the three):

* basic authentication - with curl commands it is  -u YOUR_EMAIL:YOUR_TOKEN (can be any:YOUR_TOKEN too)
<br>**OR**
* Parameters - parameters in the query string (which is usually more convinient with browser's debugging) - api_url?api_token=YOUR_TOKEN
<br>**OR**
* Custom Header - PTToken -  for example <br>
`export TOKEN='YOUR_TOKEN'`<br>
`curl -X POST -H "PTToken: $TOKEN" \`<br>
` -H "Content-Type:application/json" \`<br>
`-d '{"data": { "type": "requirements", "attributes": {"name": "one", "author-id": 6178, "priority": "highest"}  } }' \`<br>
`"https://api.practitest.com/api/v2/projects/4823/requirements.json"`<br>


<aside class="success">
You must replace <code>YOUR_TOKEN</code> with your custom API token.
<br><br>

**Developer email is not authenticated by the API, and not required anymore. You can put there anything (even the word 'any'). But if you have errors, or bad syntax, the only way that we get back to you, would be if you put your valid email address. This way we can help you if we see something wrong.
</aside>

## API Token types
> This command: https://api.practitest.com/api/v2/projects.json?api_token=xx&developer_email=admin%40pt.com&page[number]=1&page[size]=2", returns JSON structured like below:

```json
{
  "data": [
    {
      "id": "4581",
      "type": "projects",
      "attributes": {
        "name": "Sanity check (Rails 4)",
        "created-at": "2016-07-28T13:34:51Z",
        "automation-support": false,
        "enable-delete-issues": false,
        "time-management-support": false
      }
    },
    {
      "id": "4578",
      "type": "projects",
      "attributes": {
        "name": "Sanity Check (screen captures)",
        "created-at": "2016-06-20T11:32:52Z",
        "automation-support": false,
        "enable-delete-issues": true,
        "time-management-support": false
      }
    }
  ],
  "links": {
    "self": "https://api.practitest.com/api/v2/projects.json?api_token=afb913899fc295e809255fbdb4fbc1fb37296250&developer_email=admin%40pt.com&page%5Bnumber%5D=1&page%5Bsize%5D=2",
    "next": "https://api.practitest.com/api/v2/projects.json?api_token=afb913899fc295e809255fbdb4fbc1fb37296250&developer_email=admin%40pt.com&page%5Bnumber%5D=2&page%5Bsize%5D=2",
    "last": "https://api.practitest.com/api/v2/projects.json?api_token=afb913899fc295e809255fbdb4fbc1fb37296250&developer_email=admin%40pt.com&page%5Bnumber%5D=3&page%5Bsize%5D=2"
  },
  "meta": {
    "current-page": 1,
    "next-page": 2,
    "prev-page": null,
    "total-pages": 3,
    "total-count": 5
  }
}
```

PractiTest supports two types of API Tokens: **Personal API Tokens (PAT), and Account tokens**.


### Personal Api Token - PAT
This token type was introduced in May 2020, and is being gradually supported in all the API resources; for each resource, there is a separate description for this token type.

When working with the Personal API Token, each user has its own token.  PractiTest will provide access to API resources based on the PAT user’s permissions. By default, actions made with PAT will be recorded as the PAT owner’s actions (unless Impersonation is in use)

For example -> in the <a href="#projects">Projects API request</a>, with Account Token, you may list all projects in the account; PAT allows you to show only the projects in the account that the specific user has access to.

If 'Impersonation' is enabled by the account owner to the PAT user, the PAT user will have the ability to impersonate someone else with this token. For example:

* When using PAT the author-id parameter is not required anymore, and the default author is the owner of the PAT.   When working with an impersonation enabled PAT this user can override the author-id to any other value.
* For entities that have history (such as Issues, Tests, and Requirements), any changes made by the PAT user will show his name under the ‘User’ column (in the history). An Impersonation PAT user can define that user attribute.

This doesn’t give the impersonating PAT user the ability to override permissions. For example: If a PAT user is not permitted to change an issue according to the project workflow, he will not be able to do so, no matter if the user has the impersonation attribute enabled or not.


### Account API Token
This is the most powerful API token type, with access to all projects in the account, and all the API resources described in this document.

<aside class="warning">
It is less recommended to use the Account API Token since it has the permissions to do everything within the Account.
</aside>
