
# Public Facing Devices - Query #1

### [YouTube Explanation with Demonstration (Pending)](https://www.youtube.com/@BeaconPulse)

## Overview  
This Microsoft Sentinel KQL query identifies workstations that are publicly accessible, not flagged as public-facing by Microsoft Defender for Endpoint (MDE)**. It extracts their public IP addresses, maps them to their respective ASNs using an external dataset, and filters out specific network ranges based on organizational needs.

The query further analyzes network activity for these public-facing devices by summarizing various event types, such as:  
- **Successful and failed connections**  
- **DNS and SSL inspections**  
- **Inbound and outbound traffic**  
- **Network signature inspections**  

Additionally, it enriches the results with **geolocation data** based on public IP addresses.  

---

## Key Features  
✔️ **Identifies publicly accessible workstation devices**  
✔️ **Enriches public IP addresses with ASN and ISP details**  
✔️ **Summarizes network events (connection attempts, failures, and inspections)**  
✔️ **Maps IP addresses to geolocation data for further analysis**  
✔️ **Filters out enterprise-reserved IP ranges (`PublicIP !startswith "<ENTERPRISE_IP_PREFIX>"`)**  
✔️ **Excludes specific ASN name ranges (`CIDRASNName !startswith "<EXCLUDED_ASN_NAME>"`)**  
✔️ **Sorts results by total log counts for prioritization**  

---

## Usage Instructions  
1. **Modify the query placeholders:**  
   - Replace `<ENTERPRISE_IP_PREFIX>` with your organization’s public IP prefix.  
   - Replace `<EXCLUDED_ASN_NAME>` with the ASN or network name to exclude (e.g., `"DNIC"` for Department of Defense networks).  

2. **Run the query in Microsoft Sentinel.**  

3. **Export the results as a `.csv` file.**  

4. **Rename the file as:**  

```commandline
let CIDRASN = (externaldata (CIDR:string, CIDRASN:int, CIDRASNName:string)  
['https://firewalliplists.gypthecat.com/lists/kusto/kusto-cidr-asn.csv.zip']  
with (ignoreFirstRecord=true));  
let Non_DNIC_PublicIPs = DeviceInfo  
| where TimeGenerated > ago(90d) 
| where DeviceType == "Workstation"  
| where OnboardingStatus == "Onboarded"  
| where PublicIP !startswith "<ENTERPRISE_IP_PREFIX>" 
| summarize count() by PublicIP  
| evaluate ipv4_lookup(CIDRASN, PublicIP, CIDR, return_unmatched=false)  
| where CIDRASNName !startswith "<EXCLUDED_ASN_NAME>"  
| distinct PublicIP;  
DeviceNetworkEvents  
| where TimeGenerated > ago(90d) 
| where LocalIP in(Non_DNIC_PublicIPs)  
| summarize   
    TotalLogs = count(),  
    RemoteIPs = make_set(RemoteIP),  
    ConnectionAcknowledged_Count = countif(ActionType == "ConnectionAcknowledged"),   
    ConnectionSuccess_Count = countif(ActionType == "ConnectionSuccess"),  
    DnsConnectionInspected_Count = countif(ActionType == "DnsConnectionInspected"),  
    ConnectionAttempt_Count = countif(ActionType == "ConnectionAttempt"),  
    SSLConnection_Count = countif(ActionType == "SslConnectionInspected"),  
    ConnectionFailed_Count = countif(ActionType == "ConnectionFailed"),  
    InboundConnectionAccepted_Count = countif(ActionType == "InboundConnectionAccepted"),  
    ListeningConnectionCreated_Count = countif(ActionType == "ListeningConnectionCreated"),  
    IcmpConnectionInspected_Count = countif(ActionType == "IcmpConnectionInspected"),  
    HttpConnectionInspected_Count = countif(ActionType == "HttpConnectionInspected"),  
    NetworkSignatureInspected_Count = countif(ActionType == "NetworkSignatureInspected")  
    by DeviceName, LocalIP  
| evaluate ipv4_lookup(CIDRASN, LocalIP, CIDR, return_unmatched=true)  
| extend GeoLocation = geo_info_from_ip_address(LocalIP) 
| project-reorder DeviceName, LocalIP, GeoLocation, CIDRASNName, TotalLogs  
| sort by TotalLogs  
| where DeviceName == "DEVICE_NAME" 
```
