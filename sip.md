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