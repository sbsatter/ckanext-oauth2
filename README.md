OAuth2 CKAN extension
=====================

[![Build Status](https://travis-ci.org/conwetlab/ckanext-oauth2.svg?branch=master)](https://travis-ci.org/conwetlab/ckanext-oauth2)
[![Coverage Status](https://coveralls.io/repos/github/conwetlab/ckanext-oauth2/badge.svg?branch=master)](https://coveralls.io/github/conwetlab/ckanext-oauth2?branch=master)

The OAuth2 extension allows site visitors to login through a custom OAuth2 server like Auth0 and can fetch user roles from another external service.

**Note**: This extension is being tested in CKAN 2.6, 2.7, 2.8 and 2.9. These are therefore considered as the supported versions


## Installation

To install the plugin, **enter your virtualenv** and install the package using `pip` as follows:

```
pip install git+https://github.com/sbsatter/ckanext-oauth2.git
```

Add the following to your CKAN `.ini` (generally `/etc/ckan/default/production.ini` or `/etc/ckan/production.ini`) file:

```
ckan.plugins = oauth2 <other-plugins>

## GPML OAuth2 Config (Preferred)

ckan.oauth2.register_url = https://digital.gpmarinelitter.org/signup
ckan.oauth2.reset_url = https://digital.gpmarinelitter.org/login
ckan.oauth2.edit_url = https://digital.gpmarinelitter.org/profile/
ckan.oauth2.authorization_endpoint = https://unep-gpml.eu.auth0.com/authorize
ckan.oauth2.token_endpoint = https://unep-gpml.eu.auth0.com/oauth/token
ckan.oauth2.profile_api_url = https://unep-gpml.eu.auth0.com/userinfo
ckan.oauth2.client_id =
ckan.oauth2.client_secret =
ckan.oauth2.scope = openid profile email
ckan.oauth2.rememberer_name = auth_tkt
ckan.oauth2.profile_api_user_field = sub
ckan.oauth2.profile_api_fullname_field = name
ckan.oauth2.profile_api_mail_field = email
ckan.oauth2.authorization_header = Authorization
ckan.oauth2.sysadmin_group_name = sysadmin
ckan.oauth2.jwt_enable = true
ckan.oauth2.stakeholder_api_token =

## DEFAULT OAuth2 configuration

ckan.oauth2.register_url = https://YOUR_OAUTH_SERVICE/users/sign_up
ckan.oauth2.reset_url = https://YOUR_OAUTH_SERVICE/users/password/new
ckan.oauth2.edit_url = https://YOUR_OAUTH_SERVICE/settings
ckan.oauth2.authorization_endpoint = https://YOUR_OAUTH_SERVICE/authorize
ckan.oauth2.token_endpoint = https://YOUR_OAUTH_SERVICE/token
ckan.oauth2.profile_api_url = https://YOUR_OAUTH_SERVICE/user
ckan.oauth2.client_id = YOUR_CLIENT_ID
ckan.oauth2.client_secret = YOUR_CLIENT_SECRET
ckan.oauth2.scope = profile other.scope
ckan.oauth2.rememberer_name = auth_tkt
ckan.oauth2.profile_api_user_field = JSON_FIELD_TO_FIND_THE_USER_IDENTIFIER
ckan.oauth2.profile_api_fullname_field = JSON_FIELD_TO_FIND_THE_USER_FULLNAME
ckan.oauth2.profile_api_mail_field = JSON_FIELD_TO_FIND_THE_USER_MAIL
ckan.oauth2.authorization_header = OAUTH2_HEADER
ckan.oauth2.profile_api_groupmembership_field = JSON_FIELD_TO_FIND_USER_ROLES_IN_ORGANIZATIONS
ckan.oauth2.sysadmin_group_name = NAME_OF_THE_SYSADMIN_GROUP
```

The ``profile_api_groupmembership_field`` should be a list of strings or JSON objects with the ``org`` field representing the organization, and ``role`` representing the role. The first case is for compatibility wirth the original plugin. May contain the ``sysadmin_group_name`` value to map the user into the CKAN sysadmin role.

The role value should be one of ``admin``, ``editor``, or ``member``. If the value is different, it will be cast to ``member``.

You can also use environment variables to configure this plugin, the name of the environment variables are:

- `CKAN_OAUTH2_REGISTER_URL`
- `CKAN_OAUTH2_RESET_URL`
- `CKAN_OAUTH2_EDIT_URL`
- `CKAN_OAUTH2_AUTHORIZATION_ENDPOINT`
- `CKAN_OAUTH2_TOKEN_ENDPOINT`
- `CKAN_OAUTH2_PROFILE_API_URL`
- `CKAN_OAUTH2_CLIENT_ID`
- `CKAN_OAUTH2_CLIENT_SECRET`
- `CKAN_OAUTH2_SCOPE`
- `CKAN_OAUTH2_REMEMBERER_NAME`
- `CKAN_OAUTH2_PROFILE_API_USER_FIELD`
- `CKAN_OAUTH2_PROFILE_API_FULLNAME_FIELD`
- `CKAN_OAUTH2_PROFILE_API_MAIL_FIELD`
- `CKAN_OAUTH2_AUTHORIZATION_HEADER`
- `CKAN_OAUTH2_PROFILE_API_GROUPMEMBERSHIP_FIELD`
- `CKAN_OAUTH2_SYSADMIN_GROUP_NAME`


* The callback URL that you should set on your OAuth 2.0 is: `https://YOUR_CKAN_INSTANCE/oauth2/callback`, replacing `YOUR_CKAN_INSTANCE` by the machine and port where your CKAN instance is running.

## Pending items
- On multiple sessions, previous sessions are discarded. Support to add multiple sessions.
- When use is logged out from frontend API, user needs to be logged out of CKAN as well. Frontend needs to adapt the following:
  - Frontend needs to logout using CKAN, instead of Auth0 API
  - Frontend should redirect the user to: /user/external-logout
  - On successful logout, the user is redirected back to the Single Page Application

## Credits

Based on the fork from [Conwetlab](https://github.com/conwetlab/ckanext-oauth2) and  [SCC Digital Hub](https://github.com/scc-digitalhub/ckanext-oauth2)
