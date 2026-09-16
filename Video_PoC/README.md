# Description:
I have found Broken Access Control — Incorrect Authorization (CWE-863) in   Squadeno – Club Sports Manager 1.11.0 plugin affected Quick-Edit ("inline-save"). 

Summary: Section and Age groups should be only read only according to plugin functions from low privileged user and it should be editable by only administrator. However there is vulnerability that low privileged user can edit sections of sport club. Meaning there is only UI based protection not full protection.

# Steps-to-Reproduce
Install wordpress and latest Squadeno Club sports Manager plugin

1) Login as low privileged user from localhost:8088/wp-login.php
2) Confirm UI hides and does not allow editing ( I upload video)
3) As low privileged user get your nonce -> Club Sports → Sports and find  GET /wp-admin/edit.php?post_type=csmgr_sportart reqest from burp suite
4) Send that request to burp repeater then find nonce from response, which should be <input type="hidden" id="_inline_edit" name="_inline_edit" value="ccbc33b43f" /> <- copy that value
5) In burp suite history send POST /wp-admin/admin-ajax.php request to repeater and change body into:

action=inline-save&_inline_edit=YOUR_NONCEf&post_type=csmgr_sportart&post_ID=17&post_title=Running+Club&post_status=publish&tax_input[csmgr_sparte][]=14&tax_input[csmgr_altersgruppe][]=16

6) Check the result that section "Athletics" changed into "Wellbeings"

IMPACT: Incorrect authorization, UI hides change but it can be changed from forging request.
