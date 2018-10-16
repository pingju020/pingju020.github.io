# [](#消息流)消息流


## [](#一、 注册：)一、 注册：

### [](#1.1 SEND:)1.1 SEND:

REGISTER sip:47.98.109.96 SIP/2.0

Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-10348572;rport

From: <sip:1011@47.98.109.96:50620>;tag=529397402

To: <sip:1011@47.98.109.96:50620>

Contact: <sip:1011@192.168.1.107:62118;transport=udp>;expires=30;+g.oma.sip-im;language="en,fr";+g.3gpp.smsip;+g.oma.sip-im.large-message;audio;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-application.ims.iari.gsma-vs";+g.3gpp.cs-voice

Call-ID: f7724003-7111-8ddc-deba-133107508dc6

CSeq: 1990331795 REGISTER

Content-Length: 0

Max-Forwards: 70

Authorization: Digest username="1011",realm="47.98.109.96",nonce="",uri="sip:47.98.109.96",response=""

Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER

Privacy: none

P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000

User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)

P-Preferred-Identity: <sip:1011@47.98.109.96:50620>

Supported: path

###[]()1.2 RECV:

SIP/2.0 401 Unauthorized
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-10348572;rport=28555;received=112.25.222.170
From: <sip:1011@47.98.109.96:50620>;tag=529397402
To: <sip:1011@47.98.109.96:50620>;tag=mQ2g4ev73re4e
Call-ID: f7724003-7111-8ddc-deba-133107508dc6
CSeq: 1990331795 REGISTER
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
WWW-Authenticate: Digest realm="47.98.109.96", nonce="b54499e4-9083-4853-a1c4-d9b0756af9a2", stale=true, algorithm=MD5, qop="auth"
Content-Length: 0

###[]() 1.3 Send:

REGISTER sip:47.98.109.96 SIP/2.0
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1433452147;rport
From: <sip:1011@47.98.109.96:50620>;tag=529397402
To: <sip:1011@47.98.109.96:50620>
Contact: <sip:1011@192.168.1.107:62118;transport=udp>;expires=30;+g.oma.sip-im;language="en,fr";+g.3gpp.smsip;+g.oma.sip-im.large-message;audio;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-application.ims.iari.gsma-vs";+g.3gpp.cs-voice
Call-ID: f7724003-7111-8ddc-deba-133107508dc6
CSeq: 1990331796 REGISTER
Content-Length: 0
Max-Forwards: 70
Authorization: Digest username="1011",realm="47.98.109.96",nonce="b54499e4-9083-4853-a1c4-d9b0756af9a2",uri="sip:47.98.109.96",response="e55ca998d27ee109798c60b2ef646ff3",algorithm=MD5,cnonce="b8877d7c2d75cd7c67ec9ab96d9c55ea",qop=auth,nc=00000001
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)
P-Preferred-Identity: <sip:1011@47.98.109.96:50620>
Supported: path


### [](#1.4 RECV:)1.4 RECV:

SIP/2.0 200 OK
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1433452147;rport=28555;received=112.25.222.170
From: <sip:1011@47.98.109.96:50620>;tag=529397402
To: <sip:1011@47.98.109.96:50620>;tag=N0U959cB114pa
Call-ID: f7724003-7111-8ddc-deba-133107508dc6
CSeq: 1990331796 REGISTER
Contact: <sip:1011@192.168.1.107:62118;transport=udp>;expires=30
Date: Tue, 28 Aug 2018 10:14:19 GMT
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Content-Length: 0##[]()二、 呼叫：


###[]()2.1 SEND：

INVITE sip:1014@47.98.109.96 SIP/2.0
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1013684476;rport
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>
Contact: \<sip:1011@192.168.1.107:62118;transport=udp>;+g.oma.sip-im;language="en,fr";+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 INVITE
Content-Type: application/sdp
Content-Length: 651
Max-Forwards: 70
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)
P-Preferred-Identity: <sip:1011@47.98.109.96:50620>
Supported: 100rel

v=0
o=doubango 1983 678902 IN IP4 192.168.1.107
s=-
c=IN IP4 192.168.1.107
t=0 0
a=tcap:1 RTP/AVPF
m=audio 54898 RTP/AVP 8 0 3 111 101
a=ptime:20
a=minptime:1
a=maxptime:255
a=silenceSupp:off - - - -
a=rtpmap:8 PCMA/8000/1
a=rtpmap:0 PCMU/8000/1
a=rtpmap:3 GSM/8000/1
a=rtpmap:111 opus/48000/2
a=fmtp:111 maxplaybackrate=16000; sprop-maxcapturerate=16000; stereo=0; sprop-stereo=0; useinbandfec=0; usedtx=0
a=rtpmap:101 telephone-event/8000/1
a=fmtp:101 0-16
a=pcfg:1 t=1
a=sendrecv
a=rtcp-mux
a=ssrc:3992734018 cname:(null)
a=ssrc:3992734018 mslabel:6994f7d1-6ce9-4fbd-acfd-84e5131ca2e2
a=ssrc:3992734018 label:doubango@audio


### [](#2.2 RECV:)2.2 RECV:SIP/2.0 100 Trying
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1013684476;rport=28555;received=112.25.222.170
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 INVITE
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Content-Length: 0


### [](#2.3 RECV:)2.2 RECV:


SIP/2.0 200 OK
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1013684476;rport=28555;received=112.25.222.170
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>;tag=payU06rZ173Zr
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 INVITE
Contact: <sip:1014@47.98.109.96:50620;transport=udp>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Accept: application/sdp
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Session-Expires: 120;refresher=uas
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 266
P-Asserted-Identity: "1014" <sip:1014@47.98.109.96>

v=0
o=FreeSWITCH 1535391023 1535391024 IN IP4 47.98.109.96
s=FreeSWITCH
c=IN IP4 47.98.109.96
t=0 0
m=audio 60706 RTP/AVP 8 101
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=rtcp-mux
a=rtcp:60706 IN IP4 47.98.109.96


### [](#2.4 SEND:)2.4 SEND:


ACK sip:1014@47.98.109.96:50620;transport=udp SIP/2.0
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1533658107;rport
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>;tag=payU06rZ173Zr
Contact: <sip:1011@192.168.1.107:62118;transport=udp>;+g.oma.sip-im;language="en,fr";+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 ACK
Content-Length: 0
Max-Forwards: 70
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)

### [](#2.5RECV) 2.5 RECV


:SIP/2.0 200 OK
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1013684476;rport=28555;received=112.25.222.170
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>;tag=payU06rZ173Zr
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 INVITE
Contact: <sip:1014@47.98.109.96:50620;transport=udp>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Accept: application/sdp
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Session-Expires: 120;refresher=uas
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 266
P-Asserted-Identity: "1014" <sip:1014@47.98.109.96>

v=0
o=FreeSWITCH 1535391023 1535391024 IN IP4 47.98.109.96
s=FreeSWITCH
c=IN IP4 47.98.109.96
t=0 0
m=audio 60706 RTP/AVP 8 101
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=rtcp-mux
a=rtcp:60706 IN IP4 47.98.109.96


### [](#2.6 SEND:)2.6 SEND:


 ACK sip:1014@47.98.109.96:50620;transport=udp SIP/2.0
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1237483276;rport
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>;tag=payU06rZ173Zr
Contact: <sip:1011@192.168.1.107:62118;transport=udp>;+g.oma.sip-im;language="en,fr";+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 ACK
Content-Length: 0
Max-Forwards: 70
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)
### [](#2.7 RECV:)2.7 RECV:

SIP/2.0 200 OK
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-1013684476;rport=28555;received=112.25.222.170
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>;tag=payU06rZ173Zr
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 INVITE
Contact: <sip:1014@47.98.109.96:50620;transport=udp>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Accept: application/sdp
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Session-Expires: 120;refresher=uas
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 266
P-Asserted-Identity: "1014" <sip:1014@47.98.109.96>

v=0
o=FreeSWITCH 1535391023 1535391024 IN IP4 47.98.109.96
s=FreeSWITCH
c=IN IP4 47.98.109.96
t=0 0
m=audio 60706 RTP/AVP 8 101
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=rtcp-mux
a=rtcp:60706 IN IP4 47.98.109.96

### [](#2.8 SEND:)2.8 SEND:


ACK sip:1014@47.98.109.96:50620;transport=udp SIP/2.0
Via: SIP/2.0/UDP 192.168.1.107:62118;branch=z9hG4bK-377827425;rport
From: <sip:1011@47.98.109.96:50620>;tag=334081213
To: <sip:1014@47.98.109.96>;tag=payU06rZ173Zr
Contact: <sip:1011@192.168.1.107:62118;transport=udp>;+g.oma.sip-im;language="en,fr";+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Call-ID: dda811eb-0664-dae3-31bb-8b8e12741336
CSeq: 1740661807 ACK
Content-Length: 0
Max-Forwards: 70
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)










### [](#2.9 RECV:INVITE:) 2.9 RECV:INVITE

RECV:INVITE sip:1011@192.168.2.2:63365;transport=udp SIP/2.0
Via: SIP/2.0/UDP 47.97.174.58:50600;rport;branch=z9hG4bK013vy9H2Zcr3H
Route: <sip:1011@112.25.222.170:20732>;transport=udp
Max-Forwards: 69
From: "1010" <sip:201@172.16.99.166>;tag=F125S6v0Z12Kj
To: <sip:1011@192.168.2.2:63365;transport=udp>
Call-ID: e1504ac5-3c91-1237-fa98-00163e08c55a
CSeq: 128668588 INVITE
Contact: <sip:mod_sofia@47.97.174.58:50600>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Privacy: none
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 243
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
X-FS-Support: update_display,send_info
P-Asserted-Identity: "1010" <sip:201@172.16.99.166>

v=0
o=FreeSWITCH 1537948692 1537948693 IN IP4 47.97.174.58
s=FreeSWITCH
c=IN IP4 47.97.174.58
t=0 0
m=audio 60612 RTP/AVP 18 101
a=rtpmap:18 G729/8000
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20


SEND: SIP/2.0 100 Trying (sent from the Transaction Layer)
Via: SIP/2.0/UDP 47.97.174.58:50600;rport=50600;branch=z9hG4bK013vy9H2Zcr3H
From: "1010"<sip:201@172.16.99.166>;tag=F125S6v0Z12Kj
To: <sip:1011@192.168.2.2:63365;transport=udp>
Call-ID: e1504ac5-3c91-1237-fa98-00163e08c55a
CSeq: 128668588 INVITE
Content-Length: 0


SEND: SIP/2.0 180 Ringing
Via: SIP/2.0/UDP 47.97.174.58:50600;rport=50600;branch=z9hG4bK013vy9H2Zcr3H
From: "1010"<sip:201@172.16.99.166>;tag=F125S6v0Z12Kj
To: <sip:1011@192.168.2.2:63365;transport=udp>;tag=269453516
Contact: <sip:1011@192.168.2.2:63365;transport=udp>
Call-ID: e1504ac5-3c91-1237-fa98-00163e08c55a
CSeq: 128668588 INVITE
Content-Length: 0
Allow: ACK, BYE, CANCEL, INVITE, MESSAGE, NOTIFY, OPTIONS, PRACK, REFER, UPDATE


SEND: SIP/2.0 200 OK
Via: SIP/2.0/UDP 47.97.174.58:50600;rport=50600;branch=z9hG4bK013vy9H2Zcr3H
From: "1010"<sip:201@172.16.99.166>;tag=F125S6v0Z12Kj
To: <sip:1011@192.168.2.2:63365;transport=udp>;tag=269453516
Contact: <sip:1011@192.168.2.2:63365;transport=udp>
Call-ID: e1504ac5-3c91-1237-fa98-00163e08c55a
CSeq: 128668588 INVITE
Content-Type: application/sdp
Content-Length: 429
Allow: ACK, BYE, CANCEL, INVITE, MESSAGE, NOTIFY, OPTIONS, PRACK, REFER, UPDATE

v=0
o=doubango 1983 678902 IN IP4 192.168.2.2
s=-
c=IN IP4 192.168.2.2
t=0 0
m=audio 10492 RTP/AVP 18 101
a=ptime:20
a=minptime:1
a=maxptime:255
a=silenceSupp:off - - - -
a=rtpmap:18 g729/8000/1
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000/1
a=fmtp:101 0-16
a=sendrecv
a=ssrc:1933585261 cname:(null)
a=ssrc:1933585261 mslabel:6994f7d1-6ce9-4fbd-acfd-84e5131ca2e2
a=ssrc:1933585261 label:doubango@audio



RECV:ACK sip:1011@192.168.2.2:63365;transport=udp SIP/2.0
Via: SIP/2.0/UDP 47.97.174.58:50600;rport;branch=z9hG4bK1aXN0425vNepD
Max-Forwards: 70
From: "1010" <sip:201@172.16.99.166>;tag=F125S6v0Z12Kj
To: <sip:1011@192.168.2.2:63365;transport=udp>;tag=269453516
Call-ID: e1504ac5-3c91-1237-fa98-00163e08c55a
CSeq: 128668588 ACK
Contact: <sip:mod_sofia@47.97.174.58:50600>
Content-Length: 0


RECV:BYE sip:1011@192.168.2.2:63365;transport=udp SIP/2.0
Via: SIP/2.0/UDP 47.97.174.58:50600;rport;branch=z9hG4bK2Kpe2ZK9Sy48r
Max-Forwards: 70
From: "1010" <sip:201@172.16.99.166>;tag=F125S6v0Z12Kj
To: <sip:1011@192.168.2.2:63365;transport=udp>;tag=269453516
Call-ID: e1504ac5-3c91-1237-fa98-00163e08c55a
CSeq: 128668589 BYE
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Reason: Q.850;cause=16;text="NORMAL_CLEARING"
Content-Length: 0
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000


SEND: SIP/2.0 200 OK
Via: SIP/2.0/UDP 47.97.174.58:50600;rport=50600;branch=z9hG4bK2Kpe2ZK9Sy48r
From: "1010"<sip:201@172.16.99.166>;tag=F125S6v0Z12Kj
To: <sip:1011@192.168.2.2:63365;transport=udp>;tag=269453516
Contact: <sip:1011@192.168.2.2:63365;transport=udp>
Call-ID: e1504ac5-3c91-1237-fa98-00163e08c55a
CSeq: 128668589 BYE
Content-Length: 0


### [](#2.10) 2.10

SEND: INVITE sip:1010@47.97.174.58 SIP/2.0
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1077035790;rport
From: <sip:1011@47.97.174.58:50600>;tag=491833803
To: <sip:1010@47.97.174.58>
Contact: <sip:1011@192.168.2.2:63365;transport=udp>;+g.oma.sip-im;language="en,fr";+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Call-ID: 99be41c1-baff-5070-3245-19a9d1d89fe9
CSeq: 295751628 INVITE
Content-Type: application/sdp
Content-Length: 474
Max-Forwards: 70
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)
P-Preferred-Identity: <sip:200@54.67.77.123:5060>
Supported: 100rel

v=0
o=doubango 1983 678902 IN IP4 192.168.2.2
s=-
c=IN IP4 192.168.2.2
t=0 0
a=tcap:1 RTP/AVPF
m=audio 18776 RTP/AVP 18 101
a=ptime:20
a=minptime:1
a=maxptime:255
a=silenceSupp:off - - - -
a=rtpmap:18 g729/8000/1
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000/1
a=fmtp:101 0-16
a=pcfg:1 t=1
a=sendrecv
a=rtcp-mux
a=ssrc:1995240198 cname:(null)
a=ssrc:1995240198 mslabel:6994f7d1-6ce9-4fbd-acfd-84e5131ca2e2
a=ssrc:1995240198 label:doubango@audio


RECV:SIP/2.0 100 Trying
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1077035790;rport=20732;received=112.25.222.170
From: <sip:1011@47.97.174.58:50600>;tag=491833803
To: <sip:1010@47.97.174.58>
Call-ID: 99be41c1-baff-5070-3245-19a9d1d89fe9
CSeq: 295751628 INVITE
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Content-Length: 0


RECV:SIP/2.0 183 Session Progress
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1077035790;rport=20732;received=112.25.222.170
From: <sip:1011@47.97.174.58:50600>;tag=491833803
To: <sip:1010@47.97.174.58>;tag=7S1r4FHNtQSmr
Call-ID: 99be41c1-baff-5070-3245-19a9d1d89fe9
CSeq: 295751628 INVITE
Contact: <sip:1010@47.97.174.58:50600;transport=udp>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Accept: application/sdp
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 289
P-Asserted-Identity: "1010" <sip:1010@47.97.174.58>

v=0
o=FreeSWITCH 1537948729 1537948730 IN IP4 47.97.174.58
s=FreeSWITCH
c=IN IP4 47.97.174.58
t=0 0
m=audio 60718 RTP/AVP 18 101
a=rtpmap:18 g729/8000
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=rtcp-mux
a=rtcp:60718 IN IP4 47.97.174.58


RECV:SIP/2.0 200 OK
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1077035790;rport=20732;received=112.25.222.170
From: <sip:1011@47.97.174.58:50600>;tag=491833803
To: <sip:1010@47.97.174.58>;tag=7S1r4FHNtQSmr
Call-ID: 99be41c1-baff-5070-3245-19a9d1d89fe9
CSeq: 295751628 INVITE
Contact: <sip:1010@47.97.174.58:50600;transport=udp>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Session-Expires: 120;refresher=uas
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 289
P-Asserted-Identity: "Outbound Call" <sip:1010@47.97.174.58>

v=0
o=FreeSWITCH 1537948729 1537948730 IN IP4 47.97.174.58
s=FreeSWITCH
c=IN IP4 47.97.174.58
t=0 0
m=audio 60718 RTP/AVP 18 101
a=rtpmap:18 g729/8000
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=rtcp-mux
a=rtcp:60718 IN IP4 47.97.174.58

SEND: ACK sip:1010@47.97.174.58:50600;transport=udp SIP/2.0
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-643006548;rport
From: <sip:1011@47.97.174.58:50600>;tag=491833803
To: <sip:1010@47.97.174.58>;tag=7S1r4FHNtQSmr
Contact: <sip:1011@192.168.2.2:63365;transport=udp>;+g.oma.sip-im;language="en,fr";+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Call-ID: 99be41c1-baff-5070-3245-19a9d1d89fe9
CSeq: 295751628 ACK
Content-Length: 0
Max-Forwards: 70
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)


SEND sip:1010@47.97.174.58:50600;transport=udp SIP/2.0
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1951085328;rport
From: <sip:1011@47.97.174.58:50600>;tag=491833803
To: <sip:1010@47.97.174.58>;tag=7S1r4FHNtQSmr
Call-ID: 99be41c1-baff-5070-3245-19a9d1d89fe9
CSeq: 295751629 BYE
Content-Length: 0
Max-Forwards: 70
Accept-Contact: *;+g.oma.sip-im
Accept-Contact: *;language="en,fr"
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)
P-Preferred-Identity: <sip:200@54.67.77.123:5060>


RECV:SIP/2.0 200 OK
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1951085328;rport=20732;received=112.25.222.170
From: <sip:1011@47.97.174.58:50600>;tag=491833803
To: <sip:1010@47.97.174.58>;tag=7S1r4FHNtQSmr
Call-ID: 99be41c1-baff-5070-3245-19a9d1d89fe9
CSeq: 295751629 BYE
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Content-Length: 0
###[](#2.11)2.11

SEND: INVITE sip:1010@47.97.174.58 SIP/2.0
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1211225053;rport
From: <sip:1011@47.97.174.58:50600>;tag=1631759813
To: <sip:1010@47.97.174.58>
Contact: <sip:1011@192.168.2.2:63365;transport=udp>;+g.oma.sip-im;language="en,fr";+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
Call-ID: 21252ba3-6621-eeb8-ebbc-d72295795fd8
CSeq: 441294702 INVITE
Content-Type: application/sdp
Content-Length: 471
Max-Forwards: 70
Accept-Contact: *;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-service.ims.icsi.mmtel"
P-Preferred-Service: urn:urn-7:3gpp-service.ims.icsi.mmtel
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)
P-Preferred-Identity: <sip:200@54.67.77.123:5060>
Supported: 100rel

v=0
o=doubango 1983 678902 IN IP4 192.168.2.2
s=-
c=IN IP4 192.168.2.2
t=0 0
a=tcap:1 RTP/AVPF
m=audio 31600 RTP/AVP 18 101
a=ptime:20
a=minptime:1
a=maxptime:255
a=silenceSupp:off - - - -
a=rtpmap:18 g729/8000/1
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000/1
a=fmtp:101 0-16
a=pcfg:1 t=1
a=sendrecv
a=rtcp-mux
a=ssrc:975277425 cname:(null)
a=ssrc:975277425 mslabel:6994f7d1-6ce9-4fbd-acfd-84e5131ca2e2
a=ssrc:975277425 label:doubango@audio


RECV:SIP/2.0 100 Trying
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1211225053;rport=20732;received=112.25.222.170
From: <sip:1011@47.97.174.58:50600>;tag=1631759813
To: <sip:1010@47.97.174.58>
Call-ID: 21252ba3-6621-eeb8-ebbc-d72295795fd8
CSeq: 441294702 INVITE
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Content-Length: 0
 

RECV:SIP/2.0 183 Session Progress
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1211225053;rport=20732;received=112.25.222.170
From: <sip:1011@47.97.174.58:50600>;tag=1631759813
To: <sip:1010@47.97.174.58>;tag=By6UBvm3eUjZp
Call-ID: 21252ba3-6621-eeb8-ebbc-d72295795fd8
CSeq: 441294702 INVITE
Contact: <sip:1010@47.97.174.58:50600;transport=udp>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Accept: application/sdp
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 289
P-Asserted-Identity: "1010" <sip:1010@47.97.174.58>

v=0
o=FreeSWITCH 1537948752 1537948753 IN IP4 47.97.174.58
s=FreeSWITCH
c=IN IP4 47.97.174.58
t=0 0
m=audio 60710 RTP/AVP 18 101
a=rtpmap:18 g729/8000
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=rtcp-mux
a=rtcp:60710 IN IP4 47.97.174.58


RECV:SIP/2.0 603 Decline
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1211225053;rport=20732;received=112.25.222.170
Max-Forwards: 70
From: <sip:1011@47.97.174.58:50600>;tag=1631759813
To: <sip:1010@47.97.174.58>;tag=By6UBvm3eUjZp
Call-ID: 21252ba3-6621-eeb8-ebbc-d72295795fd8
CSeq: 441294702 INVITE
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Reason: Q.850;cause=16;text="NORMAL_CLEARING"
Content-Length: 0
P-Asserted-Identity: "1010" <sip:1010@47.97.174.58>


SEND: ACK sip:1010@47.97.174.58 SIP/2.0
Via: SIP/2.0/UDP 192.168.2.2:63365;branch=z9hG4bK-1211225053;rport
From: <sip:1011@47.97.174.58:50600>;tag=1631759813
To: <sip:1010@47.97.174.58>;tag=By6UBvm3eUjZp
Call-ID: 21252ba3-6621-eeb8-ebbc-d72295795fd8
CSeq: 441294702 ACK
Content-Length: 0
Max-Forwards: 70

#[]()




*[DOUBANGO INFO]: 

RECV:INVITE sip:1010@192.168.2.4:55594;transport=udp SIP/2.0
Via: SIP/2.0/UDP 47.97.174.58;rport;branch=z9hG4bK22yy4r29Q0QtK
Route: <sip:1010@112.25.222.170:22861>;transport=udp
Max-Forwards: 69
From: "1011" <sip:1011@172.16.99.166>;tag=e7H36D760KSUB
To: <sip:1010@192.168.2.4:55594;transport=udp>
Call-ID: fdbfde58-3d77-1237-fa98-00163e08c55a
CSeq: 128718004 INVITE
Contact: <sip:mod_sofia@47.97.174.58:5060>
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, presence, as-feature-event, dialog, line-seize, call-info, sla, include-session-description, presence.winfo, message-summary, refer
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 291
X-FS-Support: update_display,send_info
Remote-Party-ID: "1011" <sip:1011@172.16.99.166>;party=calling;screen=yes;privacy=off

v=0
o=FreeSWITCH 1538047484 1538047485 IN IP4 47.97.174.58
s=FreeSWITCH
c=IN IP4 47.97.174.58
t=0 0
m=audio 60652 RTP/AVP 18 0 8 101
a=rtpmap:18 G729/8000
a=fmtp:18 annexb=no
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20



*[DOUBANGO INFO]: State machine: tsip_transac_ist_Started_2_Proceeding_X_INVITE
*[DOUBANGO INFO]: 

SEND: SIP/2.0 100 Trying (sent from the Transaction Layer)
Via: SIP/2.0/UDP 47.97.174.58;rport;branch=z9hG4bK22yy4r29Q0QtK
From: "1011"<sip:1011@172.16.99.166>;tag=e7H36D760KSUB
To: <sip:1010@192.168.2.4:55594;transport=udp>
Call-ID: fdbfde58-3d77-1237-fa98-00163e08c55a
CSeq: 128718004 INVITE
Content-Length: 0




*[DOUBANGO INFO]: m_lines_count=0,
is_dtls_fingerprint_changed=0,
is_sdes_crypto_changed=0,
is_ice_enabled=0,
is_ice_restart=0,
is_ro_hold_resume_changed=0,
is_ro_provisional_final_matching=0,
is_ro_media_lines_changed=0,
is_ro_network_info_changed=0,
is_ro_loopback_address=0,
is_media_type_changed=0,
is_ro_codecs_changed=0
is_local_encoder_still_ok=0

*[DOUBANGO INFO]: tdav_consumer_audio_init()
*[DOUBANGO INFO]: Create SpeexDSP denoiser
*[DOUBANGO INFO]: Create SpeexDSP jitter buffer
*[DOUBANGO INFO]: new m_lines_count = 0 -> 1
*[DOUBANGO INFO]: RTP/RTCP manager[Begin]: Trying to bind to random ports
*[DOUBANGO INFO]: RTP/RTCP manager[End]: Trying to bind to random ports
*[DOUBANGO INFO]: State machine: s0000_Started_2_Ringing_X_iINVITE
*[DOUBANGO INFO]: State machine: tsip_transac_ist_Proceeding_2_Proceeding_X_1xx
*[DOUBANGO INFO]: 

SEND: SIP/2.0 180 Ringing
Via: SIP/2.0/UDP 47.97.174.58;rport;branch=z9hG4bK22yy4r29Q0QtK
From: "1011"<sip:1011@172.16.99.166>;tag=e7H36D760KSUB
To: <sip:1010@192.168.2.4:55594;transport=udp>;tag=77470699
Contact: <sip:1010@192.168.2.4:55594;transport=udp>
Call-ID: fdbfde58-3d77-1237-fa98-00163e08c55a
CSeq: 128718004 INVITE
Content-Length: 0
Allow: ACK, BYE, CANCEL, INVITE, MESSAGE, NOTIFY, OPTIONS, PRACK, REFER, UPDATE



SEND: SIP/2.0 200 OK
Via: SIP/2.0/UDP 47.97.174.58;rport;branch=z9hG4bK22yy4r29Q0QtK
From: "1011"<sip:1011@172.16.99.166>;tag=e7H36D760KSUB
To: <sip:1010@192.168.2.4:55594;transport=udp>;tag=77470699
Contact: <sip:1010@192.168.2.4:55594;transport=udp>
Call-ID: fdbfde58-3d77-1237-fa98-00163e08c55a
CSeq: 128718004 INVITE
Content-Type: application/sdp
Content-Length: 428
Allow: ACK, BYE, CANCEL, INVITE, MESSAGE, NOTIFY, OPTIONS, PRACK, REFER, UPDATE

v=0
o=doubango 1983 678902 IN IP4 192.168.2.4
s=-
c=IN IP4 192.168.2.4
t=0 0
m=audio 9960 RTP/AVP 18 101
a=ptime:20
a=minptime:1
a=maxptime:255
a=silenceSupp:off - - - -
a=rtpmap:18 g729/8000/1
a=fmtp:18 annexb=no
a=rtpmap:101 telephone-event/8000/1
a=fmtp:101 0-16
a=sendrecv
a=ssrc:1243003200 cname:(null)
a=ssrc:1243003200 mslabel:6994f7d1-6ce9-4fbd-acfd-84e5131ca2e2
a=ssrc:1243003200 label:doubango@audio



*[DOUBANGO INFO]: max_bw_up=2147483647 kpbs, max_bw_down=2147483647 kpbs, congestion_ctrl_enabled=1, media_type=2
*[DOUBANGO INFO]: AudioUnit producer prepared(channels=1, rate=8000, ptime=20)
*[DOUBANGO INFO]: AudioUnit consumer prepared (ptime=20, channels=1, rate=8000)
*[DOUBANGO INFO]: SO_RCVBUF = 131070, SO_SNDBUF = 131070
*[DOUBANGO INFO]: rtp.remote_ip=47.97.174.58, rtp.remote_port=60652, rtp.local_fd=18
*[DOUBANGO INFO]: rtcp.remote_ip=47.97.174.58, rtcp.remote_port=60653, rtcp.local_fd=19
*[DOUBANGO INFO]: rtcp.local_ip=192.168.2.4, rtcp.local_port=9961, rtcp.local_fd=19
*[DOUBANGO INFO]: Socket added
*[DOUBANGO INFO]: tsk_timer_manager_start
*[DOUBANGO INFO]: Timer manager already running
*[DOUBANGO INFO]: Socket added
*[DOUBANGO INFO]: Transport::run(RTP/RTCP Manager) - enter
*[DOUBANGO INFO]: Starting [RTP/RTCP Manager] server with IP {192.168.2.4} on port {9960} with fd {18}...
*[DOUBANGO INFO]: __CFSocketCallBack(fd=18), callbackType=8
*[DOUBANGO INFO]: __CFSocketCallBack(fd=19), callbackType=8
*[DOUBANGO INFO]: 

RECV:ACK sip:1010@192.168.2.4:55594;transport=udp SIP/2.0
Via: SIP/2.0/UDP 47.97.174.58;rport;branch=z9hG4bK3BrQ6KKDN9DDF
Max-Forwards: 70
From: "1011" <sip:1011@172.16.99.166>;tag=e7H36D760KSUB
To: <sip:1010@192.168.2.4:55594;transport=udp>;tag=77470699
Call-ID: fdbfde58-3d77-1237-fa98-00163e08c55a
CSeq: 128718004 ACK
Contact: <sip:mod_sofia@47.97.174.58:5060>
Content-Length: 0




*[DOUBANGO INFO]: State machine: tsip_transac_ist_Accepted_2_Accepted_iACK
*[DOUBANGO INFO]: AudioUnit consumer: in.channels=1, out.channles=0, in.rate=8000, out.rate=0, ptime=20
*[DOUBANGO INFO]: Frame duration=23
*[DOUBANGO INFO]: AudioUnit consumer initialized and ready
*[DOUBANGO INFO]: AudioUnit producer(0) or consumer(1) not ready yet ...delaying start
*[DOUBANGO INFO]: AudioUnit consumer started
*[DOUBANGO INFO]: AudioUnit producer: channels=1, rate=8000, ptime=20
*[DOUBANGO INFO]: Frame duration=23
*[DOUBANGO INFO]: AudioUnit producer initialized
*[DOUBANGO INFO]: AudioUnit producer started
*[DOUBANGO INFO]: Audio denoiser to be opened(record_frame_size_samples=160, record_sampling_rate=8000, record_channels=1, playback_frame_size_samples=160, playback_sampling_rate=8000, playback_channels=1)
*[DOUBANGO INFO]: SpeexDSP: Noise supp enabled
warning: The VAD has been replaced by a hack pending a complete rewrite
*[DOUBANGO INFO]: State machine: x0000_Connected_2_Connected_X_iACK
*[DOUBANGO INFO]: Open speex jb (ptime=20, rate=8000)
*[DOUBANGO INFO]: Default Jitter buffer margin=0
*[DOUBANGO INFO]: Default Jitter max late rate=4
*[DOUBANGO INFO]: OnDialogEvent(Dialog connected, 5)
*[DOUBANGO INFO]: State machine: tsip_transac_nict_Completed_2_Terminated_X_timerK
*[DOUBANGO INFO]: === NICT terminated ===
*[DOUBANGO INFO]: *** NICT destroyed ***
*[DOUBANGO INFO]: State machine: tsip_dialog_register_Connected_2_InProgress_X_oRegister
*[DOUBANGO INFO]: State machine: tsip_transac_nict_Started_2_Trying_X_send
*[DOUBANGO INFO]: 

SEND: REGISTER sip:47.97.174.58 SIP/2.0
Via: SIP/2.0/UDP 192.168.2.4:55594;branch=z9hG4bK-1412302526;rport
From: <sip:1010@47.97.174.58:5060>;tag=406472700
To: <sip:1010@47.97.174.58:5060>
Contact: <sip:1010@192.168.2.4:55594;transport=udp>;expires=30;+g.oma.sip-im;language="en,fr";+g.3gpp.smsip;+g.oma.sip-im.large-message;audio;+g.3gpp.icsi-ref="urn%3Aurn-7%3A3gpp-application.ims.iari.gsma-vs";+g.3gpp.cs-voice
Call-ID: fbda8abb-5374-77ef-bbb8-7cf725112755
CSeq: 1848820495 REGISTER
Content-Length: 0
Max-Forwards: 70
Allow: INVITE, ACK, CANCEL, BYE, MESSAGE, OPTIONS, NOTIFY, PRACK, UPDATE, REFER
Privacy: none
P-Access-Network-Info: ADSL;utran-cell-id-3gpp=00000000
User-Agent: IM-client/OMA1.0 ios-ngn-stack/v00 (doubango r000)
P-Preferred-Identity: <sip:1008@127.0.0.1:1024>




*[DOUBANGO INFO]: OnDialogEvent((un)REGISTER request successfully sent., 3)
*[DOUBANGO INFO]: 

RECV:SIP/2.0 200 OK
Via: SIP/2.0/UDP 192.168.2.4:55594;branch=z9hG4bK-1412302526;rport=22861;received=112.25.222.170
From: <sip:1010@47.97.174.58:5060>;tag=406472700
To: <sip:1010@47.97.174.58:5060>;tag=gS4ma48DU550j
Call-ID: fbda8abb-5374-77ef-bbb8-7cf725112755
CSeq: 1848820495 REGISTER
Contact: <sip:1010@192.168.2.4:55594;transport=udp>;expires=30
Date: Fri, 28 Sep 2018 04:15:52 GMT
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Content-Length: 0




*[DOUBANGO INFO]: State machine: tsip_transac_nict_Trying_2_Completed_X_200_to_699
*[DOUBANGO INFO]: State machine: tsip_dialog_register_InProgress_2_Connected_X_2xx
**[DOUBANGO WARN]: function: "tdav_speex_jitterbuffer_get()" 
file: "/Users/yangjuanping/Documents/work-code/VoSATProject/VoSAT/doubango/tinyDAV/src/audio/tdav_speex_jitterbuffer.c" 
line: "212" 
MSG: Too much missing audio pkts
*[DOUBANGO INFO]: State machine: tsip_transac_nict_Completed_2_Terminated_X_timerK
*[DOUBANGO INFO]: === NICT terminated ===
*[DOUBANGO INFO]: *** NICT destroyed ***
*[DOUBANGO INFO]: 

RECV:BYE sip:1010@192.168.2.4:55594;transport=udp SIP/2.0
Via: SIP/2.0/UDP 47.97.174.58;rport;branch=z9hG4bK4mHg8e4gjj4Za
Max-Forwards: 70
From: "1011" <sip:1011@172.16.99.166>;tag=e7H36D760KSUB
To: <sip:1010@192.168.2.4:55594;transport=udp>;tag=77470699
Call-ID: fdbfde58-3d77-1237-fa98-00163e08c55a
CSeq: 128718005 BYE
User-Agent: FreeSWITCH-mod_sofia/1.6.19~64bit
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY, PUBLISH, SUBSCRIBE
Supported: timer, path, replaces
Reason: Q.850;cause=16;text="NORMAL_CLEARING"
Content-Length: 0




*[DOUBANGO INFO]: State machine: tsip_transac_nist_Started_2_Trying_X_request
*[DOUBANGO INFO]: State machine: x0000_Any_2_Terminated_X_iBYE
*[DOUBANGO INFO]: State machine: tsip_transac_nist_Trying_2_Completed_X_send_200_to_699
*[DOUBANGO INFO]: 

SEND: SIP/2.0 200 OK
Via: SIP/2.0/UDP 47.97.174.58;rport;branch=z9hG4bK4mHg8e4gjj4Za
From: "1011"<sip:1011@172.16.99.166>;tag=e7H36D760KSUB
To: <sip:1010@192.168.2.4:55594;transport=udp>;tag=77470699
Contact: <sip:1010@192.168.2.4:55594;transport=udp>
Call-ID: fdbfde58-3d77-1237-fa98-00163e08c55a
CSeq: 128718005 BYE
Content-Length: 0




*[DOUBANGO INFO]: === INVITE Dialog terminated ===
*[DOUBANGO INFO]: State machine: tsip_transac_ist_Any_2_Terminated_X_cancel
*[DOUBANGO INFO]: === IST terminated ===
*[DOUBANGO INFO]: *** IST destroyed ***
*[DOUBANGO INFO]: State machine: tsip_transac_nist_Any_2_Terminated_X_cancel
*[DOUBANGO INFO]: === NIST terminated ===
*[DOUBANGO INFO]: tmedia_session_mgr_stop()
*[DOUBANGO INFO]: AudioUnit producer deinitialized
*[DOUBANGO INFO]: AudioUnit producer stoppped
*[DOUBANGO INFO]: trtp_manager_stop()
*[DOUBANGO INFO]: Transport::run(RTP/RTCP Manager) - exit
*[DOUBANGO INFO]: Stopped [RTP/RTCP Manager] server with IP {192.168.2.4} on port {9960}...
*[DOUBANGO INFO]: Socket removed: 19
*[DOUBANGO INFO]: Socket removed: 18
*[DOUBANGO INFO]: CloseSocket(18)
*[DOUBANGO INFO]: *** Transport (RTP/RTCP Manager) destroyed ***
*[DOUBANGO INFO]: CloseSocket(19)
*[DOUBANGO INFO]: *** AudioUnit Instance destroyed ***
*[DOUBANGO INFO]: AudioUnit consumer deinitialized
*[DOUBANGO INFO]: AudioUnit consumer stoppped
*[DOUBANGO INFO]: === NIST terminated ===
*[DOUBANGO INFO]: *** INVITE Dialog destroyed ***
*[DOUBANGO INFO]: *** NIST destroyed ***
*[DOUBANGO INFO]: OnDialogEvent(Call Terminated, 5)
*[DOUBANGO INFO]: *** tdav_session_audio_t destroyed ***
*[DOUBANGO INFO]: AudioUnit producer deinitialized
*[DOUBANGO INFO]: AudioUnit producer stoppped
*[DOUBANGO INFO]: trtp_manager_stop()
*[DOUBANGO INFO]: AudioUnit consumer deinitialized
*[DOUBANGO INFO]: AudioUnit consumer stoppped
*[DOUBANGO INFO]: AudioUnit consumer deinitialized
*[DOUBANGO INFO]: *** SpeexDSP denoiser destroyed ***
*[DOUBANGO INFO]: *** SpeexDSP jb destroyed ***
*[DOUBANGO INFO]: *** AudioUnit Consumer destroyed ***
*[DOUBANGO INFO]: AudioUnit producer deinitialized
*[DOUBANGO INFO]: *** AudioUnit Producer destroyed ***
*[DOUBANGO INFO]: *** RTP manager destroyed ***
*[DOUBANGO INFO]: *** Audio session destroyed ***
*[DOUBANGO INFO]: *** SIP Session destroyed ***
SCP_SDK:rcvPeerData,38, m_rcvSeq = 7895, rcvmsg.seq = 7896
SCP_SDK:SendTo,103, SendTo=6, errno=9
SCP_SDK:#0;496225;MessageInHandler.cxx;127;1;0;5;30
2018-09-28 12:16:00.007162+0800 VoSAT[7388:834864] Status bar could not find cached time string image. Rendering in-process.

