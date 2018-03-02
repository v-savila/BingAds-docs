# AccountsApp
This is the top-level object for managing multiple accounts in the Bing Ads system.

# Methods
|Method Name|Return Type|Description|
|-|-|-
[accounts](#accounts)|[BingAdsAccountSelector](./BingAdsAccountSelector)|Returns a selector of all accounts accessible to the current user under the current customer shell.<br />
[select(BingAdsAccount account)](#select~bingadsaccount-account~)|void|Selects a [BingAdsAccount](./BingAdsAccount) as the the current account on which to perform operations.
&nbsp;|&nbsp;|&nbsp;

## <a name="accounts"></a>accounts
Returns a selector of all accounts accessible to the current user under the current customer shell.

### Returns:
|Type|Description|
|-|-
[BingAdsAccountSelector](./BingAdsAccountSelector)|The selector of accounts managed by this account.
&nbsp;|&nbsp;
## <a name="select~bingadsaccount-account~"></a>select(BingAdsAccount account)
Selects a [BingAdsAccount](./BingAdsAccount) as the the current account on which to perform operations.
### Arguments:
|Name|Type|Description|
|-|-|-
account|[BingAdsAccount](./BingAdsAccount)|The account in whose context the script will continue executing.<br />
&nbsp;|&nbsp;|&nbsp;
### Returns:
|Type|Description|
|-|-
void|
&nbsp;|&nbsp;