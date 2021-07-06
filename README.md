# Host-to-Host. User's manual



[TOC]

##  Introduction

The document describes the rules an electornic document management system for corporations and banks. That is necessary for making payments, currency conversion, currency control functions and providing banks with reports to corporations related to the above services and products based on the international standard ISO 20022.



## Request for bank statement



### URI

HTTP POST: /API/v1/ISO20022/Statements



### XSD scheme
camt.060.001.03



### Scheme description

| Description                              | Link                            | Comment/Example                                              |
| ---------------------------------------- | ------------------------------- | ------------------------------------------------------------ |
| **Absolute path Document.*AcctRptgReq*** | --                              | --                                                           |
| Unique message ID                        | GrpHdr.**MsgId**                | Statement request is made by virtue of identifier            |
| Date and time the message was created    | GrpHdr.**CreDtTm**              | Date format YYYY-MM-DDTHH:MM:SS 2018-11-27T17:13:45          |
| Unique package ID with request           | RptgReq.**Id**                  | Statement request for each account number is required in a separate package |
| Get data by type Statement               | RptgReq.**ReqdMsgNmId**         |                                                              |
| Account number                           | RptgReq.Acct.Id.Othr.**Id**     |                                                              |
| Period Start Date                        | RptgReq.RptgPrd.FrToDt.**FrDt** |                                                              |
| Period End Date                          | RptgReq.RptgPrd.FrToDt.**ToDt** |                                                              |
| Period Start Time                        | RptgReq.RptgPrd.FrToTm.**FrTm** | 00:00:00                                                     |
| Period End Time                          | RptgReq.RptgPrd.FrToTm.**ToTm** | 24:00:00                                                     |
| "Show all operations"                    | RptgReq.RptgPrd.**Tp**          | Constant 'ALLL'                                              |
| Name of the company                      | RptgReq.AcctOwnr.Pty.**Nm**     |                                                              |



### ANSWER

|HTTP code |Type |Body |Description|
| ------------- |------------- |------------- |-------------|
|200|OK|-|Statement request was accepted without errors|
|401|Err|Wrong username or password||
|401|Err|User was not found||
|401|Err|User is disabled||
|401|Err|Warrant is expired||
|401|Err|Warrant was not found||
|401|Err|Warrant service error||
|406|Err|User does not have permission||
|406|Err|Signature is missing||
|406|Err|Certificate is not valid||
|406|Err|Signature is not valid||
|406|Err|Internal sign check state error||
|502|Err|Bad Gateway||
|503|Err|Service Unavailable||
|504|Err|Gateway Timeout||
|500|Err|Internal Error||
|400|Err|FrDt/ToDt is in the future||
|400|Err|Date period is not specified||
|400|Err|FrDt is later than ToDt||
|400|Err|MsgId is not unique||
|400|Err|XML does not match schema||



### Statement request example

```xml
<?xml version="1.0" encoding="utf-8"?>
<Document xmlns="urn:iso:std:iso:20022:tech:xsd:camt.060.001.03" xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
  <AcctRptgReq>
    <GrpHdr>
      <MsgId>77866e0f08374910bb1cc6f77a664727</MsgId>
      <CreDtTm>2019-02-06T15:27:10</CreDtTm>
    </GrpHdr>
    <RptgReq>
      <Id>2d60068df7644b8587ef440299ed53c6</Id>
      <ReqdMsgNmId>HMQSTASCF</ReqdMsgNmId>
      <Acct>
        <Id>
          <Othr>
            <Id>40702810901300013000</Id>
          </Othr>
        </Id>
      </Acct>
      <AcctOwnr>
        <Pty>
          <Nm>ООО Диски</Nm>
        </Pty>
      </AcctOwnr>
      <RptgPrd>
        <FrToDt>
          <FrDt>2019-02-06</FrDt>
          <ToDt>2019-02-06</ToDt>
        </FrToDt>
        <FrToTm>
          <FrTm>00:00:00</FrTm>
          <ToTm>24:00:00</ToTm>
        </FrToTm>
        <Tp>ALLL</Tp>
      </RptgPrd>
    </RptgReq>
    <SplmtryData>
      <Envlp>
        <SgntrSt>
          <ds:Signature>
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="urn:ietf:params:xml:ns:cpxmlsec:algorithms:gostr34102001-gostr3411" />
              <ds:Reference URI="">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="urn:ietf:params:xml:ns:cpxmlsec:algorithms:gostr3411" />
                <ds:DigestValue>Mv8l/zkKhXIgc+NYS5AahbSAxcR4fITrtwi72Wsid8k=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>cUvrSEG06N8434QLqap1JjCFks2p+zYFrmi6KDduU8aJL3dShmoBKqUlloU+9lOu
29r9/DTaeaJtiWhkihEsSw==</ds:SignatureValue>
            <ds:KeyInfo>
              <ds:X509Data>
                <ds:X509Certificate>MIIDbjCCAx2gAwIBAgITEgAvPdF2qLVO81iKLwAAAC890TAIBgYqhQMCAgMwfzEj
MCEGCSqGSIb3DQEJARYUc3VwcG9ydEBjcnlwdG9wcm8ucnUxCzAJBgNVBAYTAlJV
MQ8wDQYDVQQHEwZNb3Njb3cxFzAVBgNVBAoTDkNSWVBUTy1QUk8gTExDMSEwHwYD
VQQDExhDUllQVE8tUFJPIFRlc3QgQ2VudGVyIDIwHhcNMTgxMTE1MDk0NDE2WhcN
MTkwMjE1MDk1NDE2WjB9MRswGQYJKoZIhvcNAQkBFgxtYWlsQG1haWwucnUxFDAS
BgNVBAMMC0VWUkFaUkVHL1RUMQ0wCwYDVQQLDARkZXB0MQwwCgYDVQQKDANOUkQx
DTALBgNVBAcMBGNpdHkxDzANBgNVBAgMBnJlZ2lvbjELMAkGA1UEBhMCUlUwYzAc
BgYqhQMCAhMwEgYHKoUDAgIkAAYHKoUDAgIeAQNDAARAjuTSuHIiQAL4J0GYGowk
NHjHM95GScbUpd5cuD0MU7qJDj8BpP95dD9dbc5chWq/+AUoVh2mvIRwqp3wqG
wqOCAXAwggFsMA4GA1UdDwEB/wQEAwIE8DATBgNVHSUEDDAKBggrBgEFBQcDAjAd
BgNVHQ4EFgQU9C6wJ7PrpufrQalBt7nMe1PQeuIwHwYDVR0jBBgwFoAUFTF8sI0a
3mbXFZxJUpcXJLkBeoMwWQYDVR0fBFIwUDBOoEygSoZIaHR0cDovL3Rlc3RjYS5j
cnlwdG9wcm8ucnUvQ2VydEVucm9sbC9DUllQVE8tUFJPJTIwVGVzdCUyMENlbnRl
ciUyMDIuY3JsMIGpBggrBgEFBQcBAQSBnDCBmTBhBggrBgEFBQcwAoZVaHR0cDov
L3Rlc3RjYS5jcnlwdG9wcm8ucnUvQ2VydEVucm9sbC90ZXN0LWNhLTIwMTRfQ1JZ
UFRPLVBSTyUyMFRlc3QlMjBDZW50ZXIlMjAyLmNydDA0BggrBgEFBQcwAYYoaHR0
cDovL3Rlc3RjYS5jcnlwdG9wcm8ucnUvb2NzcC9vY3NwLnNyZjAIBgYqhQMCAgMD
QQBhwelOmwMKApnFmrtTiegDOv3wF4imE+k193xizjiO8MI1+JG32WI4ZUF1wXUL
mFk/7avO4MNBzZAt8NxB/nmW</ds:X509Certificate>
              </ds:X509Data>
            </ds:KeyInfo>
          </ds:Signature>
        </SgntrSt>
      </Envlp>
    </SplmtryData>
  </AcctRptgReq>
</Document>
```



## Receiving a statement



### URI

<p>GET: /API/v1/ISO20022/Statements/&lt;MsgId&gt;</p>



### XSD scheme

camt.053.001.05



### Scheme description

<table>
<lable>Headline (GrpHdr)</lable>
<thead>
  <tr><td>Description</td><td>Link</td></tr>
</thead>
<tbody>
<tr><td colspan = "2"><b>Common path Document.BkToCstmrStmt.GrpHdr.</b></td></tr>
<tr><td>Unique message ID</td>
<td><b>MsgId</b></td></tr>
<tr><td>Date and Time the message was created</td>
<td><b>MsgIdCreDtTm</b></td></tr>
<tr><td>Name of the Recipient</td>
<td>MsgRcpt.<b>Nm</b></td></tr>
<tr><td>TIN of the recipient of the statement</td>
<td>MsgRcpt.Id.OrgId.Othr.<b>Id</b></td></tr>
<tr><td>Page Number (pagination)</td>
<td>MsgPgntn.<b>PgNb</b></td></tr>
<tr><td>Last page indicator (pagination)</td>
<td>MsgPgntn.<b>LastPgInd</b></td></tr>
</tbody>
</table>
<table>
<lable>.Statement Section (Stmt)</lable>
<thead>
  <tr><td>Description</td><td>Link</td><td>Comment/Example</td></tr>
</thead>
<tbody>
<tr><td colspan = "3">Common path <b>Document.BkToCstmrStmt.Stmt</b></td></tr>
<tr><td>Bank statement ID</td>
<td><b>Id</b></td>
<td></td>
</tr>
<tr><td>Date and time of the statement generation</td>
<td><b>CreDtTm</b></td>
<td></td>
</tr>
<tr><td>Period Start Date</td>
<td>FrToDt.<b>FrDtTm</b></td>
<td></td>
</tr>
<tr><td>End date</td>
<td>FrToDt.<b>ToDtTm</b></td>
<td></td>
</tr>
<tr><td>Account number</td>
<td>Acct.Id.Othr.<b>Id</b></td>
<td>20 characters</td>
</tr>
<tr><td>Name of the account holder</td>
<td>Acct.Ownr.<b>Nm</b></td>
<td>Name of the client (organization)</td>
</tr>
<tr><td>TIN of the account holder</td>
<td>Acct.Ownr.Id.OrgId.Othr.<b>Id</b></td>
<td>TIN of the Client</td>
</tr>
<tr><td>ISO data type code for TIN</td>
<td>Acct.Ownr.Id.OrgId.Othr.SchmeNm.<b>Cd</b></td>
<td>Constant 'TXID'</td>
</tr>
<tr><tr><td colspan = "3"><b>Bank details</b></td></tr>
<tr><td>BIC of the bank serving the account</td>
<td>Acct.Svcr.FinInstnId.<b>BICFI</b></td>
<td>Set SWIFTs for currency documents. Information on BICs, see "BIC Bank / branch serving the account".</td>
</tr>
<tr><td>Designation of the Russian settlement system</td>
<td>Acct.Svcr.FinInstnId.ClrSysMmbId.ClrSysId.<b>Cd</b></td>
<td>Constant 'RUCBC'</td>
</tr>
<tr><td>BIC of the bank / branch serving the account</td>
<td>Acct.Svcr.FinInstnId.ClrSysMmbId.<b>MmbId</b></td>
<td></td>
</tr>
<tr><td>Name of the bank / branch serving the account</td>
<td>Acct.Svcr.FinInstnId.<b>Nm</b></td>
<td></td>
</tr>
<tr><td>Address of the bank / branch serving the account</td>
<td>Acct.Svcr.FinInstnId.PstlAdr.<b>AdrLine</b></td>
<td></td>
</tr>
<tr><td>TIN of the bank / branch serving the account</td>
<td>Acct.Svcr.FinInstnId.Othr.<b>Id</b></td>
<td></td>
</tr>
<tr><tr><td colspan = "3"><b>Balance Information</b></td></tr>
<tr>
<td>Balance Type (Incoming) (1 block)</td>
<td>Bal.Tp.CdOrPrtry.<b>Cd</b></td>
<td>Constant "OPBD"</td>
</tr>
<tr>
<td>Incoming balance (account currency)</td>
<td>Bal.<b>Amt@Ccy</b></td>
<td></td>
</tr>
 <tr>
<td>Credit indicator ('CRDT')</td>
<td>Bal.<b>CdtDbtInd</b></td>
<td>Constant "CRDT"</td>
</tr>
 <tr>
<td>Balance date</td>
<td>Bal.Dt.<b>Dt</b></td>
<td></td>
</tr>
<tr>
<td>Balance Type (Outgoing) (2 block)</td>
<td>Bal.Tp.CdOrPrtry.<b>Cd</b></td>
<td>Constant "CLBD"</td>
</tr>
<tr>
<td>Outgoing balance (валюта счета)</td>
<td>Bal.<b>Amt@Ccy</b></td>
<td></td>
</tr>
<tr>
<td>Debit Indicator ('DBIT')</td>
<td>Bal.<b>CdtDbtInd</b></td>
<td>Constant DBIT</td>
</tr>
<tr>
<td>Balance date</td>
<td>Bal.Dt.<b>Dt</b></td>
<td></td>
</tr>
<tr>
<td>Credit Turnovers</td>
<td>TxsSummry.TtlCdtNtries.<b>Sum</b></td>
<td></td>
</tr>
<tr>
<td>Debit Turnovers</td>
<td>TxsSummry.TtlDbtNtries.<b>Sum</b></td>
<td></td>
</tr>
</tbody>
</table>

### Answer

If the request is works, the service returns an XML document corresponding to the format camt.053.001.05. Otherwise, the service returns one of the following messages:

| HTTP code | Type | Body                                                         | Description |
| --------- | ---- | ------------------------------------------------------------ | ----------- |
| 200       | OK   | The request is still being processed. Try later              |             |
| 401       | Err  | Wrong username or password                                   |             |
| 401       | Err  | User was not found                                           |             |
| 403       | Err  | User is disabled                                             |             |
| 401       | Err  | Warrant service error                                        |             |
| 502       | Err  | Bad Gateway                                                  |             |
| 503       | Err  | Service Unavailable                                          |             |
| 504       | Err  | Gateway Timeout                                              |             |
| 500       | Err  | Internal Error                                               |             |
| 400       | Err  | Request has not been not found by MsgId                      |             |
| 500       | Err  | The statement is not final and has incorrect closing date: <current business date> |             |
| 500       | Err  | The request has not been processed due to errors. Please contact the service support. |             |

Example:

```xml
<?xml version="1.0" encoding="windows-1251" standalone="yes"?>
<Document xmlns="urn:iso:std:iso:20022:tech:xsd:camt.053.001.05" xmlns:ns2="urn:iso:std:iso:20022:tech:xsd:camt.060.001.03">
    <BkToCstmrStmt>
        <GrpHdr>
            <MsgId>3ae4ddb616f341eaad22e50a2df5ecd9</MsgId>
            <CreDtTm>2019-02-07T16:35:33.562+03:00</CreDtTm>
        </GrpHdr>
        <Stmt>
            <Id>0fdbee7ab8ca4f138d4528fd2349210a</Id>
            <CreDtTm>2019-02-07T16:35:33.562+03:00</CreDtTm>
            <FrToDt>
                <FrDtTm>2019-02-07T03:00:00</FrDtTm>
                <ToDtTm>2019-02-07T03:00:00</ToDtTm>
            </FrToDt>
            <Acct>
                <Id>
                    <Othr>
                        <Id>40702810901300013000</Id>
                    </Othr>
                </Id>
                <Ownr>
                    <Nm>АО "ДИСКИ"</Nm>
                    <Id>
                        <OrgId>
                            <Othr>
                                <Id>5036045205</Id>
                                <SchmeNm>
                                    <Cd>TXID</Cd>
                                </SchmeNm>
                            </Othr>
                        </OrgId>
                    </Id>
                </Ownr>
                <Svcr>
                    <FinInstnId>
                        <BICFI>ALFARUMMXXX</BICFI>
                        <ClrSysMmbId>
                            <ClrSysId>
                                <Cd>RUCBC</Cd>
                            </ClrSysId>
                            <MmbId>044525593</MmbId>
                        </ClrSysMmbId>
                        <Nm>ДО "ЦЕНТР ОБСЛУЖИВ.КРУПНЫХ КОРПОР.КЛИЕНТОВ" г.МОСКВА АО"АЛЬФА-БАНК"</Nm>
                        <PstlAdr>
                            <AdrLine>107078,Россия, г.Москва, ул.Маши Порываевой д.34</AdrLine>
                        </PstlAdr>
                        <Othr>
                            <Id>7728168971</Id>
                        </Othr>
                    </FinInstnId>
                </Svcr>
            </Acct>
            <Bal>
                <Tp>
                    <CdOrPrtry>
                        <Cd>OPBD</Cd>
                    </CdOrPrtry>
                </Tp>
                <Amt Ccy="RUR">238705473983.24</Amt>
                <CdtDbtInd>CRDT</CdtDbtInd>
                <Dt>
                    <Dt>2019-02-07</Dt>
                </Dt>
            </Bal>
            <Bal>
                <Tp>
                    <CdOrPrtry>
                        <Cd>CLBD</Cd>
                    </CdOrPrtry>
                </Tp>
                <Amt Ccy="RUR">238705473602.24</Amt>
                <CdtDbtInd>DBIT</CdtDbtInd>
                <Dt>
                    <Dt>2019-02-07</Dt>
                </Dt>
            </Bal>
            <TxsSummry>
                <TtlCdtNtries>
                    <Sum>0.00</Sum>
                </TtlCdtNtries>
                <TtlDbtNtries>
                    <Sum>381.00</Sum>
                </TtlDbtNtries>
            </TxsSummry>
            <Ntry>
                <Amt Ccy="RUR">51.00</Amt>
                <CdtDbtInd>DBIT</CdtDbtInd>
                <Sts>PDNG</Sts>
                <BookgDt>
                    <Dt>2019-02-07</Dt>
                </BookgDt>
                <ValDt>
                    <Dt>2019-02-07</Dt>
                </ValDt>
                <BkTxCd>
                    <Domn>
                        <Cd>PMNT</Cd>
                        <Fmly>
                            <Cd>ICDT</Cd>
                            <SubFmlyCd>NTAV</SubFmlyCd>
                        </Fmly>
                    </Domn>
                </BkTxCd>
                <NtryDtls>
                    <TxDtls>
                        <Refs>
                            <EndToEndId>8914</EndToEndId>
                        </Refs>
                        <Amt Ccy="RUR">51.00</Amt>
                        <CdtDbtInd>DBIT</CdtDbtInd>
                        <Chrgs>
                            <Rcrd>
                                <Amt Ccy="RUR">0.00</Amt>
                                <Br>CRED</Br>
                                <Agt>
                                    <FinInstnId />
                                </Agt>
                            </Rcrd>
                        </Chrgs>
                        <RltdPties>
                            <Dbtr>
                                <Nm>АО "ДИСКИ"</Nm>
                                <Id>
                                    <OrgId>
                                        <Othr>
                                            <Id>5036045205</Id>
                                            <SchmeNm>
                                                <Cd>TXID</Cd>
                                            </SchmeNm>
                                        </Othr>
                                    </OrgId>
                                </Id>
                            </Dbtr>
                            <DbtrAcct>
                                <Id>
                                    <Othr>
                                        <Id>40702810901300013000</Id>
                                    </Othr>
                                </Id>
                            </DbtrAcct>
                            <Cdtr>
                                <Nm>АО "ДИСКИ"</Nm>
                                <Id>
                                    <OrgId>
                                        <Othr>
                                            <Id>5036045205</Id>
                                            <SchmeNm>
                                                <Cd>TXID</Cd>
                                            </SchmeNm>
                                        </Othr>
                                    </OrgId>
                                </Id>
                            </Cdtr>
                            <CdtrAcct>
                                <Id>
                                    <Othr>
                                        <Id>40702810001850000513</Id>
                                    </Othr>
                                </Id>
                            </CdtrAcct>
                        </RltdPties>
                        <RltdAgts>
                            <DbtrAgt>
                                <FinInstnId>
                                    <ClrSysMmbId>
                                        <ClrSysId>
                                            <Cd>RUCBC</Cd>
                                        </ClrSysId>
                                        <MmbId>044525593</MmbId>
                                    </ClrSysMmbId>
                                    <Nm>АО "АЛЬФА-БАНК" г. Москва</Nm>
                                    <Othr>
                                        <Id>30101810200000000593</Id>
                                    </Othr>
                                </FinInstnId>
                            </DbtrAgt>
                            <CdtrAgt>
                                <FinInstnId>
                                    <ClrSysMmbId>
                                        <ClrSysId>
                                            <Cd>RUCBC</Cd>
                                        </ClrSysId>
                                        <MmbId>044525593</MmbId>
                                    </ClrSysMmbId>
                                    <Nm>АО "АЛЬФА-БАНК" г. Москва</Nm>
                                    <Othr>
                                        <Id>30101810200000000593</Id>
                                    </Othr>
                                </FinInstnId>
                            </CdtrAgt>
                            <IntrmyAgt1>
                                <FinInstnId />
                            </IntrmyAgt1>
                        </RltdAgts>
                        <Purp>
                            <Prtry>5</Prtry>
                        </Purp>
                        <RmtInf>
                            <Ustrd>Пополнение расчетного счета предприятия для текущей деятельности. НДС не облагается</Ustrd>
                        </RmtInf>
                    </TxDtls>
                </NtryDtls>
            </Ntry>
            <Ntry>
                <Amt Ccy="RUR">330.00</Amt>
                <CdtDbtInd>DBIT</CdtDbtInd>
                <Sts>PDNG</Sts>
                <BookgDt>
                    <Dt>2019-02-07</Dt>
                </BookgDt>
                <ValDt>
                    <Dt>2019-02-07</Dt>
                </ValDt>
                <BkTxCd>
                    <Domn>
                        <Cd>PMNT</Cd>
                        <Fmly>
                            <Cd>MDOP</Cd>
                            <SubFmlyCd>COMM</SubFmlyCd>
                        </Fmly>
                    </Domn>
                </BkTxCd>
                <NtryDtls>
                    <TxDtls>
                        <Refs>
                            <EndToEndId>6</EndToEndId>
                        </Refs>
                        <Amt Ccy="RUR">330.00</Amt>
                        <CdtDbtInd>DBIT</CdtDbtInd>
                        <RltdPties>
                            <Dbtr>
                                <Nm>Акционерное общество "ДИСКИ"</Nm>
                                <Id>
                                    <OrgId>
                                        <Othr>
                                            <Id>5036045205</Id>
                                            <SchmeNm>
                                                <Cd>TXID</Cd>
                                            </SchmeNm>
                                        </Othr>
                                    </OrgId>
                                </Id>
                            </Dbtr>
                            <DbtrAcct>
                                <Id>
                                    <Othr>
                                        <Id>40702810901300013000</Id>
                                    </Othr>
                                </Id>
                            </DbtrAcct>
                            <Cdtr>
                                <Nm>АО "Альфа-Банк"</Nm>
                                <Id>
                                    <OrgId>
                                        <Othr>
                                            <Id>7728168971</Id>
                                            <SchmeNm>
                                                <Cd>TXID</Cd>
                                            </SchmeNm>
                                        </Othr>
                                    </OrgId>
                                </Id>
                            </Cdtr>
                            <CdtrAcct>
                                <Id>
                                    <Othr>
                                        <Id>47423810301300025101</Id>
                                    </Othr>
                                </Id>
                            </CdtrAcct>
                        </RltdPties>
                        <RltdAgts>
                            <DbtrAgt>
                                <FinInstnId>
                                    <Nm>ЦЕНТР КРУПНЫХ КОРПОР. КЛИЕНТОВ</Nm>
                                </FinInstnId>
                            </DbtrAgt>
                        </RltdAgts>
                        <RmtInf>
                            <Ustrd>Комиссия за обслуживание р/с за период с 07ЯНВ19 по 06ФЕВ19 Согласно тарифам Банка АО "ДИСКИ"</Ustrd>
                        </RmtInf>
                    </TxDtls>
                </NtryDtls>
            </Ntry>
        </Stmt>
    </BkToCstmrStmt>
</Document>
```

## Electronic signature

An electronic signature is generated by the algorithm GOST R 34.10-2012 (256 and 512 bits) according to the XMLDSig standard. The Signature section, which contains the XMLDSig-generated EP, is placed in the SgntrSt section inside the SplmtryData section, which is designed to accommodate arbitrary data. Each Signature section contains a link to the section to be signed (s) inside the xml document. The message must be fully signed, including the section

```xml
<SplmtryData>
    <Envlp>
        <SgntrSt>
        </SgntrSt>
    </Envlp>
</SplmtryData>
```

General recommendations for generating an XMLDSig signature:

1) You can use a certified combination of the cryptographic provider CryptoPro CSP and the API from Java to it CryptoPro JavaCSP, but you must specify JavaCSP in your software

2) You can use CryptoPro JCP 2.0.
There is .jar with examples:

- samples.jar
- samples-sources.jar
- including xmlSign in its distribution.

When you sign with two keys, you should sign only the data. When you sign with the second signature, the first signature is not signed

To avoid the "UnrecoverableKeyException: Get Key failed" error, you need to transfer the keys and certificate from the * .pfx repository to the HDImageStore repository (this will be a folder with 6 * .key files), which Java distinguishes with installed CryptoPro
(more details https://www.cryptopro.ru/forum2/default.aspx?g=posts&t=8271)

Examples of implementation and signed documents: https://github.com/alfa-laboratory/iso20022-signature

### Signature Formation Example

```xml
<CstmrCdtTrfInitn>
    ...
    <SplmtryData>
        <Envlp>
            <SgntrSt>
                <Signature хmlns="http://www.w3.org/2000/09/xmldsig#">
                    {ЭП #1 …}
                </Signature>

                <Signature хmlns="http://www.w3.org/2000/09/xmldsig#">
                    {ЭП #2 …}
                </Signature>
            </SgntrSt>
        </Envlp>
    </SplmtryData>
</CstmrCdtTrfInitn>
```



### Example request summary extract with signature

```xml
<?xml version="1.0" encoding="UTF-8"?><p:Document xmlns:p="urn:iso:std:iso:20022:tech:xsd:camt.060.001.03" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:iso:std:iso:20022:tech:xsd:camt.060.001.03 xsd/camt.060.001.03.xsd ">
<p:AcctRptgReq>
 <p:GrpHdr>
   <p:MsgId>MSG_20170830_test_55</p:MsgId>
   <p:CreDtTm>2017-05-26T12:00:00</p:CreDtTm>
 </p:GrpHdr>
 <p:RptgReq>
   <p:Id>REQ_20170830_test_55</p:Id>
   <p:ReqdMsgNmId>HMQSTASCF</p:ReqdMsgNmId>
   <p:Acct>
       <p:Id>
           <p:Othr>
               <p:Id>40702810001300013144</p:Id>
           </p:Othr>
       </p:Id>
   </p:Acct>
   <p:AcctOwnr>
       <p:Pty>
           <p:Nm>ООО "Мир Технологий"</p:Nm>
       </p:Pty>
   </p:AcctOwnr>
   <p:RptgPrd>
     <p:FrToDt>
       <p:FrDt>2017-02-23</p:FrDt>
       <p:ToDt>2017-02-23</p:ToDt>
     </p:FrToDt>
    <p:FrToTm>
       <p:FrTm>00:00:00</p:FrTm>
       <p:ToTm>24:00:00</p:ToTm>
     </p:FrToTm>
     <p:Tp>ALLL</p:Tp>
   </p:RptgPrd>
 </p:RptgReq>
  <p:RptgReq>
   <p:Id>REQ_20170830_test_56</p:Id>
   <p:ReqdMsgNmId>HMQSTASCF</p:ReqdMsgNmId>
   <p:Acct>
       <p:Id>
           <p:Othr>
               <p:Id>40702810100000000921</p:Id>
           </p:Othr>
       </p:Id>
   </p:Acct>
   <p:AcctOwnr>
       <p:Pty>
           <p:Nm>ООО "Мир Технологий"</p:Nm>
       </p:Pty>
   </p:AcctOwnr>
   <p:RptgPrd>
     <p:FrToDt>
       <p:FrDt>2017-02-23</p:FrDt>
       <p:ToDt>2017-02-23</p:ToDt>
     </p:FrToDt>
    <p:FrToTm>
       <p:FrTm>00:00:00</p:FrTm>
       <p:ToTm>24:00:00</p:ToTm>
     </p:FrToTm>
     <p:Tp>ALLL</p:Tp>
   </p:RptgPrd>
 </p:RptgReq>
 <p:SplmtryData>
      <p:Envlp>
          <SgntrSt>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#" Id="sigID1">
<ds:SignedInfo>
<ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
<ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#gostr34102001-gostr3411"/>
<ds:Reference URI="">
<ds:Transforms>
<ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
<ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
</ds:Transforms>
<ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#gostr3411"/>
<ds:DigestValue>ALQVhJd+YufeR5ebo1dFcv5Fdv0eqSSNWfEOm2soDrU=</ds:DigestValue>
</ds:Reference>
</ds:SignedInfo>
<ds:SignatureValue>
S3XOmm7Mm4CnzXRng7gXTuMLuuOsQ1BcKVJz43NZHtd28hnBUR6uojAvQBE4bLhR9lxMioagvQF0
fP81BHvEUw==
</ds:SignatureValue>
<ds:KeyInfo>
<ds:X509Data>
<ds:X509Certificate>
MIIDcjCCAyGgAwIBAgITEgAewCrzlZtxaIf/EAAAAB7AKjAIBgYqhQMCAgMwfzEjMCEGCSqGSIb3
DQEJARYUc3VwcG9ydEBjcnlwdG9wcm8ucnUxCzAJBgNVBAYTAlJVMQ8wDQYDVQQHEwZNb3Njb3cx
FzAVBgNVBAoTDkNSWVBUTy1QUk8gTExDMSEwHwYDVQQDExhDUllQVE8tUFJPIFRlc3QgQ2VudGVy
IDIwHhcNMTcwNzI4MDgxNzIzWhcNMTcxMDI4MDgyNzIzWjCBgDEeMBwGCSqGSIb3DQEJARYPaXZh
bm92QGl2YW4uY29tMRYwFAYDVQQDDA1jZXJ0aWZpY2F0ZTA3MQwwCgYDVQQLDANkZXAxDDAKBgNV
BAoMA29yZzENMAsGA1UEBwwEY2l0eTEOMAwGA1UECAwFc3RhdGUxCzAJBgNVBAYTAlJVMGMwHAYG
KoUDAgITMBIGByqFAwICIwEGByqFAwICHgEDQwAEQHMSlGtvqSHl25faa9kJ2M4PckhXZZReBvRX
dbHwXWB0omjM6mwi2Kt72GdVcg/JJKXG6/xEyOdyXfgGMIsjAYOjggFwMIIBbDAOBgNVHQ8BAf8E
BAMCBsAwEwYDVR0lBAwwCgYIKwYBBQUHAwIwHQYDVR0OBBYEFBLW6VyxD2CMbq8kKTZE/7G+dHCF
MB8GA1UdIwQYMBaAFBUxfLCNGt5m1xWcSVKXFyS5AXqDMFkGA1UdHwRSMFAwTqBMoEqGSGh0dHA6
Ly90ZXN0Y2EuY3J5cHRvcHJvLnJ1L0NlcnRFbnJvbGwvQ1JZUFRPLVBSTyUyMFRlc3QlMjBDZW50
ZXIlMjAyLmNybDCBqQYIKwYBBQUHAQEEgZwwgZkwYQYIKwYBBQUHMAKGVWh0dHA6Ly90ZXN0Y2Eu
Y3J5cHRvcHJvLnJ1L0NlcnRFbnJvbGwvdGVzdC1jYS0yMDE0X0NSWVBUTy1QUk8lMjBUZXN0JTIw
Q2VudGVyJTIwMi5jcnQwNAYIKwYBBQUHMAGGKGh0dHA6Ly90ZXN0Y2EuY3J5cHRvcHJvLnJ1L29j
c3Avb2NzcC5zcmYwCAYGKoUDAgIDA0EAL9Hgr5aVBQ1FYVbEHBIjbejchr4RO09Sr05AAORWwGhv
x7BO717qODeZl3uKX4oRaJ6EIZJW0yyvE6VwnE7h+Q==
</ds:X509Certificate>
</ds:X509Data>
</ds:KeyInfo>
</ds:Signature></SgntrSt>
      </p:Envlp>
      </p:SplmtryData>
</p:AcctRptgReq>
</p:Document>
```





