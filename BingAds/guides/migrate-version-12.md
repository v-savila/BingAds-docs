---
title: "Migrating to Bing Ads API Version 12"
ms.service: "bing-ads"
ms.topic: "article"
author: "eric-urban"
ms.author: "eur"
description: Get details about migrating to Bing Ads API version 12.
---
# Migrating to Bing Ads API Version 12
> [!NOTE]
> This preview documentation is subject to change. 

> [!IMPORTANT]
> With the availability of Bing Ads API version 12, version 11 is deprecated and will sunset by October 31, 2018. 

The sections below describe changes from version 11 to version 12 of the [Ad Insight](~/ad-insight-service/ad-insight-service-reference.md), [Bulk](~/bulk-service/bulk-service-reference.md), [Campaign Management](~/campaign-management-service/campaign-management-service-reference.md), [Customer Billing](~/customer-billing-service/customer-billing-service-reference.md), [Customer Management](~/customer-management-service/customer-management-service-reference.md), and [Reporting](~/reporting-service/reporting-service-reference.md) services. Some [authentication](#authentication) updates are required for all services.

## <a name="authentication"></a>Authentication for All Services

### Breaking Changes

#### <a name="authentication-oauth-required"></a>Microsoft Account Authentication via OAuth is Required
Starting with Bing Ads API version 12, only OAuth authentication will be supported. Managed credentials i.e., the *UserName* and *Password* header elements are not supported. 

For more information on how to use OAuth with Bing Ads, see [Authentication with OAuth](authentication-oauth.md) and [Authentication With the SDKs](sdk-authentication.md).

#### <a name="authentication-multi-user"></a>Multi User Credentials
Previously one Bing Ads user could only access accounts within a single customer. You could manage accounts across multiple customers as an agency, however, you would have had to create a distinct user per customer. Now with multi-user credentials it is possible to use one Microsoft account and manage accounts across different customers. Your Microsoft account can be assigned a different user role (Super Admin, Standard User, Advertiser Campaign Manager, or Viewer) per customer that you can access. For example, your multi-user credentials grants you access to Customer A and Customer B. However, your Viewer user role for Customer A limits you from making any changes on the accounts that Belong to Customer A. But as a Super Admin for Customer B, you have full control over that customer's accounts. 

Please note the following changes from version 11 to 12 if you or one of your clients have setup [Multi-User Credentials for Customer Accounts](customer-accounts.md#multi-user). 

Starting with Bing Ads API Version 12 the multi-user credentials can access accounts across multiple customers. To switch context between customers you are required to set the CustomerId and CustomerAccountId header elements for Ad Insight Bulk, Campaign Management, and Reporting services. 
> [!NOTE]
> The CustomerId and CustomerAccountId headers are not available for the Customer Billing and Customer Management services, because they automatically detect the customer context of the current authenticated user.

The merged credentials that could authenticate via Bing Ads API Version 11 will not authenticate via Bing Ads API Version 12. 
> [!TIP]
> To confirm availability, if the credentials no longer work via the Bing Ads web application (due to being merged for multi-user credentials via another email address), then the merged Microsoft account could still authenticate via Bing Ads API Version 11 but cannot authenticate via Bing Ads API Version 12.

## <a name="adinsight"></a>Ad Insight

### Breaking Changes

#### <a name="adinsight-proxy-client"></a>Proxy Client

Update your proxy client to use the new endpoint address and namespace.

The namespace is `https://bingads.microsoft.com/AdInsight/v12`.

The production endpoint is [https://adinsight.api.bingads.microsoft.com/Api/Advertiser/AdInsight/v12/AdInsightService.svc](https://adinsight.api.bingads.microsoft.com/Api/Advertiser/AdInsight/v12/AdInsightService.svc).

The sandbox endpoint is [https://adinsight.api.sandbox.bingads.microsoft.com/Api/Advertiser/AdInsight/v12/AdInsightService.svc](https://adinsight.api.sandbox.bingads.microsoft.com/Api/Advertiser/AdInsight/v12/AdInsightService.svc).

#### <a name="adinsight-currencycode"></a>ISO Currency Codes
The *Currency* value set is renamed as [CurrencyCode](~/ad-insight-service/currencycode.md). The values are updated with ISO codes e.g., *USD* replaces *USDollar*.

The new value set is used with the [BidLandscapePoint](~/ad-insight-service/bidlandscapepoint.md), [EstimatedBidAndTraffic](~/ad-insight-service/estimatedbidandtraffic.md), and [EstimatedPositionAndTraffic](~/ad-insight-service/estimatedpositionandtraffic.md) objects. 

#### <a name="adinsight-currencycode"></a>ISO Currency Codes
In version 12 all attributes within the [KeywordIdea](~/ad-insight-service/keywordidea.md) are nillable i.e., AdImpressionShare, Competition, Relevance, Source, and SuggestedBid. If you do not request them, the [GetKeywordIdeas](~/ad-insight-service/getkeywordideas.md) operation will return nil properties in the returned [KeywordIdea](~/ad-insight-service/keywordidea.md). In addition the Competition [KeywordIdeaAttribute](~/ad-insight-service/keywordideaattribute.md) is no longer required when calling [GetKeywordIdeas](~/ad-insight-service/getkeywordideas.md). 

In version 11 if you didn't request AdImpressionShare, Relevance, Source, or SuggestedBid the [GetKeywordIdeas](~/ad-insight-service/getkeywordideas.md) operation returned zero values (AdImpressionShare=0, Relevance=0, Source=Unknown, and SuggestedBid=0), although the values should not have been used. 

#### <a name="adinsight-sunset-content"></a>Content Ad Distribution
The Content ad distribution is no longer supported in Bing Ads, and the *Content* value is removed from the [MatchType](~/ad-insight-service/matchtype.md) value set. 

## <a name="bulk"></a>Bulk

### Breaking Changes

#### <a name="bulk-proxy-client"></a>Proxy Client

Update your proxy client to use the new endpoint address and namespace.

The namespace is `https://bingads.microsoft.com/CampaignManagement/v12`.

The production endpoint is [https://bulk.api.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/BulkService.svc](https://bulk.api.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/BulkService.svc).

The sandbox endpoint is [https://bulk.api.sandbox.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/BulkService.svc](https://bulk.api.sandbox.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/BulkService.svc).


#### <a name="bulk-formatversion5"></a>Format Version 6.0

Support for Bulk file format version 5.0 is removed. Bing Ads API Version 12 only supports format version 6.0. When calling the [DownloadCampaignsByAccountIds](~/bulk-service/downloadcampaignsbyaccountids.md) and [DownloadCampaignsByCampaignIds](~/bulk-service/downloadcampaignsbycampaignids.md) operations you must specify *6.0* in the *FormatVersion* request element. When uploading a bulk file, you must specify 6.0 in the *Name* field of the [Format Version](~/bulk-service/format-version.md) record. Changes to records between format version 5.0 and 6.0 are described in more detail in the following sections.

#### <a name="bulk-sitelinkadextensions"></a>Sitelink Ad Extensions
During calendar year 2017, Bing Ads upgraded all format version 5.0 *Sitelink Ad Extension* records (contains multiple sitelinks per ad extension) to *Sitelink2 Ad Extension* objects (contains one sitelink per ad extension). 

In Bing Ads API Version 12 the format version 5.0 *Sitelink Ad Extension*, *Campaign Sitelink Ad Extension*, and *AdGroup Sitelink Ad Extension* records are removed. Likewise, the *SiteLinksAdExtension*, *CampaignSiteLinksAdExtensions*, and *AdGroupSiteLinksAdExtensions* values are removed from the [DownloadEntity](~/bulk-service/download-entity) value set.

When you migrate to version 12 remove the '2' suffix from all *Sitelink2* records i.e., use the format version 6.0 [Sitelink Ad Extension](~/bulk-service/sitelink-ad-extension.md), [Account Sitelink Ad Extension](~/bulk-service/account-sitelink-ad-extension.md), [Campaign Sitelink Ad Extension](~/bulk-service/campaign-sitelink-ad-extension.md), and [Ad Group Sitelink Ad Extension](~/bulk-service/ad-group-sitelink-ad-extension.md) records. Likewise remove the '2' suffix from each [DownloadEntity](~/bulk-service/download-entity) value in version 12 i.e., use *SitelinkAdExtensions*, *AccountSitelinkAdExtensions*, *CampaignSitelinkAdExtensions*, and *AdGroupSitelinkAdExtensions*.

#### <a name="bulk-audiencetargetingsetting"></a>Audience Targeting Setting
The *Remarketing Targeting Setting* field of an [Ad Group](~/bulk-service/ad-group.md) is renamed as *Audience Targeting Setting*. The setting is applicable for all audiences, including but not limited to remarketing lists. 

#### <a name="bulk-sunset-content"></a>Content Ad Distribution
The Content ad distribution is no longer supported in Bing Ads, and the *Content* field of an [Ad Group](~/bulk-service/ad-group.md) is removed from version 12. The ad distribution is effectively determined by the campaign type e.g., Search or Audience campaigns. 

#### <a name="bulk-sunset-cpm"></a>Cpm Pricing Model
The CPM pricing model is no longer supported in Bing Ads, and the *Pricing Model* field of an [Ad Group](~/bulk-service/ad-group.md) is removed from version 12. The *Pricing Model* field in version 11 was optional, defaulted to *Cpc*, and could only be set to *Cpc*. 

#### <a name="bulk-timezone"></a>Time Zone Updates
The following time zone values are updated for the *Time Zone* field of the [Campaign](~/bulk-service/campaign.md) record. 
-  Since Magadan is now permanently in UTC+12 time zone, the value is updated from *MagadanSolomonIslandNewCaledonia* to *SolomonIslandNewCaledonia*.
-  The value is updated from *Almaty_Novosibirsk* to *AlmatyNovosibirsk*.
-  The value is updated from *MidwayIslandand_Samoa* to *MidwayIslandAndSamoa*.

## <a name="campaign"></a>Campaign Management

### Breaking Changes

#### <a name="campaign-proxy-client"></a>Proxy Client
Update your proxy client to use the new endpoint address and namespace.

The namespace is `https://bingads.microsoft.com/CampaignManagement/v12`.

The production endpoint is [https://campaign.api.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/CampaignManagementService.svc](https://campaign.api.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/CampaignManagementService.svc).

The sandbox endpoint is [https://campaign.api.sandbox.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/CampaignManagementService.svc](https://campaign.api.sandbox.bingads.microsoft.com/Api/Advertiser/CampaignManagement/v12/CampaignManagementService.svc).


#### <a name="campaign-returnadditionalfields"></a>Return Additional Fields
The *ReturnAdditionalFields* element is removed from the [GetAdGroupsByCampaignId](~/campaign-management-service/getadgroupsbycampaignid.md),[GetAdGroupsByIds](~/campaign-management-service/getadgroupsbyids.md), [GetAudiencesByIds](~/campaign-management-service/getaudiencesbyids.md), [GetKeywordsByAdGroupId](~/campaign-management-service/getkeywordsbyadgroupid.md), [GetKeywordsByEditorialStatus](~/campaign-management-service/getkeywordsbyeditorialstatus.md),  and [GetKeywordsByIds](~/campaign-management-service/getkeywordsbyids.md) request messages, and the corresponding elements of each [AdGroup](~/campaign-management-service/adgroup.md), [Audience](~/campaign-management-service/audience.md), and [Keyword](~/campaign-management-service/keyword.md) are returned by default.

#### <a name="campaign-sitelinkadextensions"></a>Sitelink Ad Extensions
During calendar year 2017, Bing Ads upgraded all version 11 *SiteLinksAdExtension* objects (contains multiple sitelinks per ad extension) to *Sitelink2AdExtension* objects (contains one sitelink per ad extension). 

In Bing Ads API Version 12 the *SiteLinksAdExtension* and *SiteLink* objects are removed. The *Sitelink2AdExtension* object is renamed [SitelinkAdExtension](~/campaign-management-service/sitelinkadextension.md). Likewise, the *SiteLinksAdExtension* value is removed from [AdExtensionsTypeFilter](~/campaign-management-service/adextensionsstypefilter.md) value set, and the *Sitelink2AdExtension* value is renamed *Sitelink2AdExtension* (sans '2' suffix).

When you migrate to version 12 remove the '2' suffix from *Sitelink2AdExtension*, and otherwise the version 12 [SitelinkAdExtension](~/campaign-management-service/sitelinkadextension.md) interface is identical to the version 11 *Sitelink2AdExtension* object. Likewise the ad extension [Type](~/campaign-management-service/sitelinkadextension.md#type) value is *SitelinkAdExtension* when you retrieve a sitelink ad extension in version 12.

#### <a name="campaign-audiencetargetingsetting"></a>Audience Targeting Setting
The *RemarketingTargetingSetting* element of an [AdGroup](~/campaign-management-service/adgroup.md) is renamed as *AudienceTargetingSetting*. The setting is applicable for all audiences, including but not limited to remarketing lists. 

#### <a name="campaign-accountmigrationstatusinfo"></a>Account Migration Status Info
The *MigrationStatusInfo* (singular) element of an [AccountMigrationStatusesInfo](~/campaign-management-service/accountmigrationstatusesinfo.md) object is renamed as *MigrationStatusInfos* (plural). This will resolve ambiguity between the returned name and data type.  

#### <a name="campaign-expandedtextaddomain"></a>Expanded Text Ad Domain
The *DisplayUrl* element of an [ExpandedTextAd](~/campaign-management-service/expandedtextad.md) object is renamed as *Domain*.  

#### <a name="campaign-mediatype"></a>Media Type
The values returned in the *MediaType* and *Type* elements of a [Media](~/campaign-management-service/media.md) object are swapped. In version 11 the derived media type of an [Image](~/campaign-management-service/image.md) was returned in the *MediaType* element e.g., *Image*, and the aspect ratio was returned in the *Type* element e.g., *Image15x10*. In version 12 the derived media type of an [Image](~/campaign-management-service/image.md) is returned in the *Type* element e.g., *Image*, and the aspect ratio is returned in the *MediaType* element e.g., *Image15x10*. Long term this should reduce friction for clients who depend on the derived type in the *Type* element.

#### <a name="campaign-sunset-content"></a>Content Ad Distribution
The Content ad distribution is no longer supported in Bing Ads, and the *AdDistribution* element of an [AdGroup](~/campaign-management-service/adgroup.md) is removed from version 12. The ad distribution is effectively determined by the campaign type e.g., Search or Audience campaigns. 

#### <a name="campaign-sunset-cpm"></a>Cpm Pricing Model
The CPM pricing model is no longer supported in Bing Ads, and the *PricingModel* element of an [AdGroup](~/campaign-management-service/adgroup.md) is removed from version 12. The *PricingModel* element in version 11 was optional, defaulted to *Cpc*, and could only be set to *Cpc*. 

#### <a name="campaign-timezone"></a>Time Zone Updates
The following time zone values are updated for the *TimeZone* element of the [Campaign](~/campaign-management-service/campaign.md) object. 
-  Since Magadan is now permanently in UTC+12 time zone, the value is updated from *MagadanSolomonIslandNewCaledonia* to *SolomonIslandNewCaledonia*.
-  The value is updated from *Almaty_Novosibirsk* to *AlmatyNovosibirsk*.
-  The value is updated from *MidwayIslandand_Samoa* to *MidwayIslandAndSamoa*.

#### <a name="campaign-batcherrorcollectioneditorial"></a>Batch Error Collection Editorial Errors
The Appealable, DisapprovedText, Location, PublisherCountry, and ReasonCode elements are added to the [BatchErrorCollection](~/campaign-management-service/batcherrorcollection.md) object. 

## <a name="billing"></a>Customer Billing

### Breaking Changes

#### <a name="billing-proxy-client"></a>Proxy Client
Update your proxy client to use the new endpoint address and namespace.

The namespace is `https://bingads.microsoft.com/Billing/v12`.

The production endpoint is [https://clientcenter.api.bingads.microsoft.com/Api/Billing/v12/CustomerBillingService.svc](https://clientcenter.api.bingads.microsoft.com/Api/Billing/v12/CustomerBillingService.svc).


## <a name="customer"></a>Customer Management

### Breaking Changes

#### <a name="customer-proxy-client"></a>Proxy Client
Update your proxy client to use the new endpoint address and namespace.

The namespace is `https://bingads.microsoft.com/Customer/v12`.

The production endpoint is [https://clientcenter.api.bingads.microsoft.com/Api/CustomerManagement/v12/CustomerManagementService.svc](https://clientcenter.api.bingads.microsoft.com/Api/CustomerManagement/v12/CustomerManagementService.svc).

The sandbox endpoint is [https://clientcenter.api.sandbox.bingads.microsoft.com/Api/CustomerManagement/v12/CustomerManagementService.svc](https://clientcenter.api.sandbox.bingads.microsoft.com/Api/CustomerManagement/v12/CustomerManagementService.svc).

#### <a name="customer-advertiseraccount"></a>Advertiser Account
The [AdvertiserAccount](~/customer-management-service/advertiseraccount.md) object no longer derives from an *Account* base. The *Account* object is removed and its properties are moved directly to the [AdvertiserAccount](~/customer-management-service/advertiseraccount.md) object. 

Also because only one account type is supported, the *AccountType* and *ApplicationType* value sets are removed. In turn the *AccountType* element is removed from the [AdvertiserAccount](~/customer-management-service/advertiseraccount.md) object, and the *ApplicationScope* request element is removed from the [SignupCustomer](~/customer-management-service/signupcustomer.md) operation.

#### <a name="customer-advertiseraccount"></a>AutoTag Type
The *AutoTag* key and value pair is removed from the *ForwardCompatibilityMap* element of the [AdvertiserAccount](~/customer-management-service/advertiseraccount.md) object. Instead the *AutoTagType* element is added to the [AdvertiserAccount](~/customer-management-service/advertiseraccount.md). The new [AutoTagType](~/customer-management-service/autotagtype.md) values are *Inactive*, *Preserve*, and *Replace*. If you used values 0, 1, or 2 in the version 11 *AutoTag*, then you can replace them with the version 12 auto tag type as follows.

Version 11|Version 12  
---------|---------
0|Inactive
1|Preserve
2|Replace

#### <a name="customer-advertiseraccount"></a>Tracking Url Template
The *TrackingUrlTemplate* key and value pair is removed from the *ForwardCompatibilityMap* element of the [AdvertiserAccount](~/customer-management-service/advertiseraccount.md) object. Instead you can set the [AccountProperty](~/campaign-management-service/accountproperty.md) name for *TrackingUrlTemplate* via the Campaign Management service, or set the *Tracking Template* field of the [Account](~/bulk-service/account.md) record via the Bulk service.  

#### <a name="customer-currencycode"></a>ISO Currency Codes
The *CurrencyType* value set is renamed as [CurrencyCode](~/customer-management-service/currencycode.md). The values are updated with ISO codes e.g., *USD* replaces *USDollar*. The new value set is used with the [AdvertiserAccount](~/customer-management-service/advertiseraccount.md) object. 

#### <a name="customer-timezone"></a>Time Zone Updates
The following values are updated in the [TimeZoneType](~/customer-management-service/timezonetype.md) value set. 
-  Since Magadan is now permanently in UTC+12 time zone, the value is updated from *MagadanSolomonIslandNewCaledonia* to *SolomonIslandNewCaledonia*.
-  The value of *InternationalDatelineWest* (lowercase 'l' in Dateline) is updated to *InternationalDateLineWest* (uppercase 'L' in DateLine). 

## <a name="reporting"></a>Reporting

### Breaking Changes

#### <a name="reporting-proxy-client"></a>Proxy Client
Update your proxy client to use the new endpoint address and namespace.

The namespace is `https://bingads.microsoft.com/Reporting/v12`.

The production endpoint is [https://reporting.api.bingads.microsoft.com/Api/Advertiser/Reporting/v12/ReportingService.svc](https://reporting.api.bingads.microsoft.com/Api/Advertiser/Reporting/v12/ReportingService.svc).

The sandbox endpoint is [https://reporting.api.sandbox.bingads.microsoft.com/Api/Advertiser/Reporting/v12/ReportingService.svc](https://reporting.api.sandbox.bingads.microsoft.com/Api/Advertiser/Reporting/v12/ReportingService.svc).

#### <a name="reporting-downloadedcolumns"></a>Consistency Between WSDL Contract and Downloaded Report Columns
Previously there were some discrepancies between the report column value set names and the names of the columns in the downloaded reports. In Reporting API Version 12 all of the downloaded column names match the requested value. For example now when you submit a [KeywordPerformanceReportRequest](~/reporting-service/keywordperformancereportrequest.md) with the *BidStrategyType* value from the [KeywordPerformanceReportColumn](~/reporting-service/keywordperformancereportcolumn.md) value set, the column name in the downloaded report is also *BidStrategyType* in Reporting API Version 12. Previously in version 11, the column name in the downloaded report was *Bid strategy type*.

The following column names have changed in the downloaded reports from version 11 to 12.

Version 11|Version 12  
---------|---------
AudienceTargetSetting|TargetingSetting
AutoTarget|DynamicAdTarget
AutoTargetId|DynamicAdTargetId
AutoTargetStatus|DynamicAdTargetStatus
AvgCPP|AverageCpp
BenchmarkCTR|BenchmarkCtr
Bid strategy type|BidStrategyType
BusinessCatId|BusinessCategoryId
BusinessCatName|BusinessCategoryName
CallDuration|Duration
CallEndTime|EndTime
CallStartTime|StartTime
CallStatusName|CallStatus
ClickSharePct|ClickSharePercent
ClickTypeID|ClickTypeId
CountryOrRegion|Country
Device type|DeviceType
DSAFinalURL|FinalUrl
FinalAppUrl|FinalAppURL
FinalMobileUrl|FinalMobileURL
FinalUrl|FinalURL
FirstLevelCategory|Category1
Localstorecode|LocalStoreCode
NegativeKeywordListName|NegativeKeywordList
NegativeKeywordMatchTypeDesc|NegativeKeywordMatchType
NodeId|AdGroupCriterionId
ProductPartitionType|PartitionType
PTR|Ptr
Search query|SearchQuery
SecondLevelCategory|Category2
Status|CampaignStatus
TopLevelCategory|Category0


#### <a name="reporting-columnrestrictions"></a>Column Restrictions
For some reports you cannot include constrained attributes in the same report request. For example when submitting the [AccountPerformanceReportRequest](~/reporting-service/accountperformancereportrequest.md) and [AdGroupPerformanceReportRequest](~/reporting-service/adgroupperformancereportrequest.md) if you include any of the impression share performance statistics columns, then you must exclude the *BidMatchType*, *DeviceOS*, and *TopVsOther* attribute columns. Likewise, if you include any of these attribute columns, then you must exclude all of the impression share performance statistics columns.

Starting with Bing Ads API Version 12, an error will be returned if you include any constrained report column combinations. Using Bing Ads API Version 11 the report submission did not fail; however, the fields returned in the downloaded report would be 0 (zero) in place of any meaningful data.

For more details, see [Column Restrictions in Reports](reports.md#columnrestrictions).

#### <a name="reporting-reportaggregation"></a>Report Aggregation
For parity with aggregation options (unit of time) in the Bing Ads web application, Bing Ads API Version 12 enables comparable time periods that previously were not supported via Bing Ads API Version 11. The *NonHourlyReportAggregation* and *SearchQueryReportAggregation* value sets are removed, and you should use [ReportAggregation](~/reporting-service/reportaggregation.md) values for all reports.

When submitting the following report requests, you must use the [ReportAggregation](~/reporting-service/reportaggregation.md) data type instead of *NonHourlyReportAggregation* within the *ReportAggregation* element. Although the [ReportAggregation](~/reporting-service/reportaggregation.md) value set includes additional aggregation periods, unless otherwise noted below these reports are still limited to Summary, Daily, Weekly, Monthly, and Yearly aggregation. If you submit a report with an invalid aggregation period, the API will return error code 2007, ReportingServiceInvalidReportAggregation. 

Report Request|Aggregation Periods Added in Version 12  
---------|---------
[AdDynamicTextPerformanceReportRequest](~/reporting-service/addynamictextperformancereportrequest.md)|Hourly
[AdPerformanceReportRequest](~/reporting-service/adperformancereportrequest.md)|DayOfWeek, Hourly, HourOfDay
[AgeGenderDemographicReportRequest](~/reporting-service/agegenderdemographicreportrequest.md)|None
[ConversionPerformanceReportRequest](~/reporting-service/conversionperformancereportrequest.md)|None
[DestinationUrlPerformanceReportRequest](~/reporting-service/destinationurlperformancereportrequest.md)|DayOfWeek, Hourly, HourOfDay
[DSAAutoTargetPerformanceReportRequest](~/reporting-service/dsaautotargetperformancereportrequest.md)|DayOfWeek, Hourly, HourOfDay
[DSACategoryPerformanceReportRequest](~/reporting-service/dsacategoryperformancereportrequest.md)|DayOfWeek, Hourly, HourOfDay
[GeographicPerformanceReportRequest](~/reporting-service/geographicperformancereportrequest.md)|DayOfWeek, Hourly, HourOfDay
[GoalsAndFunnelsReportRequest](~/reporting-service/goalsandfunnelsreportrequest.md)|None
[PublisherUsagePerformanceReportRequest](~/reporting-service/publisherusageperformancereportrequest.md)|DayOfWeek, Hourly, HourOfDay
[ShareOfVoiceReportRequest](~/reporting-service/shareofvoicereportrequest.md)|None
[UserLocationPerformanceReportRequest](~/reporting-service/userlocationperformancereportrequest.md)|DayOfWeek, Hourly, HourOfDay

When submitting the following report requests, you must use the [ReportAggregation](~/reporting-service/reportaggregation.md) data type instead of *SearchQueryReportAggregation* within the *ReportAggregation* element.
-  [DSASearchQueryPerformanceReportRequest](~/reporting-service/dsasearchqueryperformancereportrequest.md)
-  [SearchQueryPerformanceReportRequest](~/reporting-service/searchqueryperformancereportrequest.md)

#### <a name="reporting-sunset-content"></a>Content Ad Distribution
The Content ad distribution is no longer supported in Bing Ads, and the *Content* value is removed from the [AdDistributionReportFilter](~/reporting-service/addistributionreportfilter.md), [BidMatchTypeReportFilter](~/reporting-service/bidmatchtypereportfilter.md), and [DeliveredMatchTypeReportFilter](~/reporting-service/deliveredmatchtypereportfilter.md) value sets. 

### New Features

#### <a name="reporting-reporttimeperiod"></a>More Flexible Report Time Periods
Bing Ads API version 12 now lets you choose a *TimeZone* when you submit a [ReportRequest](~/reporting-service/reportrequest.md). The time zone can help you accurately scope data for the selected date range. 

#### <a name="reporting-reporttimeperiod"></a>More Flexible Report Time Periods
Bing Ads API version 12 now lets you choose *LastFourteenDays* and *LastThirtyDays* from the [ReportTimePeriod](~/reporting-service/reporttimeperiod.md) value set when you submit a report request. 
 
#### <a name="reporting-topvsother"></a>Top Vs Other for Product Dimension Report
The *TopVsOther* column is added to the [ProductDimensionPerformanceReportColumn](~/reporting-service/productdimensionperformancereportcolumn.md). 