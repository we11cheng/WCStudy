#### iOS 查看ipa 证书到期时间

1. 将包的后缀ipa 改为zip ，解压 

2. payload 找到包  显示包内容

3. 文件夹搜索 找到以.mobileprovision为后缀的文件

4. 命令行  security cms -D -i XXX.mobileprovision

5. 可以查看签名信息,证书，创建时间，TeamName，证书过期时间，是否是企业证书（ProvisionsAllDevices这个参数是true），UUID，Version等，方便开发者查看打包的信息

6. 搜索CreationDate字段

7. 企业包 创建日期是根据bundle identify 的第一次打包时间计算，之后一年有效期。

#### 操作实测

#### 使用cat命令查看
```
~ » cd /Users/rby/Desktop/sop/Spotify破解版汉化/Payload/Spotify.app/
cat embedded.mobileprovision 
???0??1 *?H??
:<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>AppIDName</key>
	<string>ftjrApp</string>
	<key>ApplicationIdentifierPrefix</key>
	<array>
	<string>Z87Y8CVBP9</string>
	</array>
	<key>CreationDate</key>
	<date>2019-12-24T02:14:57Z</date>
	<key>Platform</key>
	<array>
		<string>iOS</string>
	</array>
	<key>IsXcodeManaged</key>
	<true/>
	<key>DeveloperCertificates</key>
	<array>
		<data>MIIFsDCCBJigAwIBAgIIIW3zt8TSW08wDQYJKoZIhvcNAQEFBQAwgZYxCzAJBgNVBAYTAlVTMRMwEQYDVQQKDApBcHBsZSBJbmMuMSwwKgYDVQQLDCNBcHBsZSBXb3JsZHdpZGUgRGV2ZWxvcGVyIFJlbGF0aW9uczFEMEIGA1UEAww7QXBwbGUgV29ybGR3aWRlIERldmVsb3BlciBSZWxhdGlvbnMgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkwHhcNMTkwNjEyMDkyMDI5WhcNMjIwNjExMDkyMDI5WjCBsTEaMBgGCgmSJomT8ixkAQEMClo4N1k4Q1ZCUDkxQjBABgNVBAMMOWlQaG9uZSBEaXN0cmlidXRpb246IFRPWU9UQSBNT1RPUiBGSU5BTkNFIChDSElOQSkgQ08uLExURDETMBEGA1UECwwKWjg3WThDVkJQOTEtMCsGA1UECgwkVE9ZT1RBIE1PVE9SIEZJTkFOQ0UgKENISU5BKSBDTy4sTFREMQswCQYDVQQGEwJVUzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOoWGK9Uff9lTjIwiqgz98hRVN6CoHXJi+544FwfSPM3aok2N4Ax1Os0IMvik41DU4fBYPd/rbVJaiN02g5sD4ikwnY64jl3jXEp1eczrwYnBpjvrrfYj5pmOIaH6CAL2LvdAY/Gxfba+32YXj2RpFkrtObKUSZTKKNul3LyMrBxI5eIithog61Zl9DB+C6F1AoD+nU5jXRmILEejqX6a8a3pg3UKdSa1kaYq0w4J05YXdf5E1foyteBCL16VX4Y9ipQUi4TJ29rFGV2S85wP0JuDU5ThIbjRcKqeyNq8N4V9Qu596JEXRQmdG5+QkWvMo3QdaW+e16MoZIW3Dy+/nsCAwEAAaOCAeMwggHfMAwGA1UdEwEB/wQCMAAwHwYDVR0jBBgwFoAUiCcXCam2GGCL7Ou69kdZxVJUo7cwPwYIKwYBBQUHAQEEMzAxMC8GCCsGAQUFBzABhiNodHRwOi8vb2NzcC5hcHBsZS5jb20vb2NzcDAyLXd3ZHIwMTCCAQ8GA1UdIASCAQYwggECMIH/BgkqhkiG92NkBQEwgfEwgcMGCCsGAQUFBwICMIG2DIGzUmVsaWFuY2Ugb24gdGhpcyBjZXJ0aWZpY2F0ZSBieSBhbnkgcGFydHkgYXNzdW1lcyBhY2NlcHRhbmNlIG9mIHRoZSB0aGVuIGFwcGxpY2FibGUgc3RhbmRhcmQgdGVybXMgYW5kIGNvbmRpdGlvbnMgb2YgdXNlLCBjZXJ0aWZpY2F0ZSBwb2xpY3kgYW5kIGNlcnRpZmljYXRpb24gcHJhY3RpY2Ugc3RhdGVtZW50cy4wKQYIKwYBBQUHAgEWHWh0dHA6Ly93d3cuYXBwbGUuY29tL2FwcGxlY2EvMBYGA1UdJQEB/wQMMAoGCCsGAQUFBwMDMB0GA1UdDgQWBBSkplNmT3DMLEp0SQbep4DH3tQkVTAOBgNVHQ8BAf8EBAMCB4AwEwYKKoZIhvdjZAYBBAEB/wQCBQAwDQYJKoZIhvcNAQEFBQADggEBAFhJ5x0Kigqx7jQ9QIrwO3/ixrKexORwSbTE4KvMyEQtLIDSsWK1AgZVOFje2+jbsTAeUfgQ7SPztTdMLED/GRK5aAHQUu97FGgBtqjQfDXBh7+w3hvDMH/VDmBCBY/sphCTvm0YME5IhtgIn1XGTHkqKX2hsAPYya9DevxiP/6mTMwikpYHWhzXnv4g3PW03ZMi1EH6ry1V8kyXtauZ7XrF0zEH0IoWc+PdyTSlijb9pOArHvZ6vK5rkza82/zAmTxaZu0K+5Sc7oUslY7E5aB+X4UzVuoaJyDSJjHH5D1VNBGvOxaXmGTdiETyBWIGyZZl13DciWzSS4dFGJ61JZs=</data>
	</array>

								
	<key>Entitlements</key>
	<dict>
				
				<key>application-identifier</key>
		<string>Z87Y8CVBP9.com.tmfcn.dealerapp</string>
				
				<key>keychain-access-groups</key>
		<array>
				<string>Z87Y8CVBP9.*</string>
		</array>
				
				<key>get-task-allow</key>
		<false/>
				
				<key>com.apple.developer.team-identifier</key>
		<string>Z87Y8CVBP9</string>

	</dict>
	<key>ExpirationDate</key>
	<date>2020-12-23T02:14:57Z</date>
	<key>Name</key>
	<string>iOS Team Inhouse Provisioning Profile: com.tmfcn.dealerapp</string>
	<key>ProvisionsAllDevices</key>
	<true/>
	<key>TeamIdentifier</key>
	<array>
		<string>Z87Y8CVBP9</string>
	</array>
	<key>TeamName</key>
	<string>TOYOTA MOTOR FINANCE (CHINA) CO.,LTD</string>
	<key>TimeToLive</key>
	<integer>365</integer>
	<key>UUID</key>
	<string>fa28bc2a-7ba4-4fe4-ad89-d76eaf800821</string>
	<key>Version</key>
	<integer>1</integer>
</dict>
</plist>??
0b1     *?H???0?۠0
   0	UUS10U

Apple Inc.1&0$U
220412174328Z0y1pple Certification Authority10U
                0	UUS10U

Apple Inc.1&0$U
?0?     *?H??  Apple Certification Authority1-0+U$Apple iPhone Certification Authority0?"0
????G???[F??!?O?!p(E`\??
dc???i??T??[?N/?k3?DL?K?	???[??dݳr???ټ??a?*??Υ^?i?d
                                                           ???PF	??尔m??????AN??e?z???n?U??)XI
c{>c??}JF?94??(e?`?W?ɉ????hR??N?ȃ??????fJ?ω?=c?)ޭ?Z?ܥ???	N?5e??
                                       ?ǟ???0??0U??0U?0?0U?4*."?9`k???w?a/1?|50U#0?+?iG?v	??k????{?tN???-??+?)?'?%http://www.apple.com/appleca/root.crl0
           ??^Br?i??k^
kK>{%޳??????=????tWܯ???
}0?*?!Y??I?nu?zц????KI???A????V?}?????QJ&??B&?Tf^`?1+kT???A?T?T??Jǻ?????F
?'????9:?p#2?kf]?M?GI?{E?Q3?tg	N?loH?,?3DkE?tKo????>%(%???Q??O?;??D,I?t?4?D???-Q?JAlXVޛ:?W?b??0??0?0y1?ό?%0*?H??
   0	UUS10U

Apple Inc.1&0$U
220412174328Z0Y1pple Certification Authority1-0+U$Apple iPhone Certification Authority0
                0	UUS10U


Apple Inc.1503U
?0?     *?H??  ,Apple iPhone OS Provisioning Profile Signing0?"0
??ٚ?????Ǚ??u!Lh0,?w?1H?D???z????GmN???q??y?Ku?h??sU?ƇQ{???j㌑?z*?ϕ^?q?(~?????O??????
2?%?6???$?dW????E!??Dv??;i?m~????C???&G?????u???~????|?=???/?xv:N|?dO*%Q?Avn???!?>]?_??
                                                                                       @?/??jJ??S?\,ļ[?={???0??0U?^k;?zGr?p(?$?/;?{??0
                                  U?00U#0??4*."?9`k???w?a/1?|500U)0'0%?#?!?http://crl.apple.com/iphone.crl0
       U?0
???VNAұ?!C?????cd0
           ?????X??A?0???ڀm!??\ܾ??9!?ġ?&????????G?*i?????k?"?+?nAL,?#??'?????d3D?;HbbT?qM?-?:??ˎ\Cql?3"< ?x??*!?siϛ???=?
                   ??s??R?¶?"?>?\??{???P:????HG?$??>L?J?g?q'??R????>???^?
??M?0?ԭ?2?P/MU?B'??rd?,?	?L?
                                    57??
0b1     *?H??                           ??1?+?\??}??0??0???0
   0	UUS10U

Apple Inc.1&0$U
350209214036Z0b1pple Certification Authority10U
                0	UUS10U

Apple Inc.1&0$U
?0?le Ro*?H??0?"0ple Certification Authority10U
?䑩	??GP??^y?-?6?WLU????Kl??"0?>?P	?A?????f?$kУ????z
?d5#KY???????P??XPg? ?ˬ, op??0??C??=?+I(??ε??^??=?:???   ?G?[?73??M?i??r?]?_??%?U?M]
?b??q?GSU??/A????p??LE~LkP?A??tb                      ?!.t?<
                                ?A?3???0X?Z2?h???es?g^e?I?v?3e?w??-??z0?v0U?0U?0?0U+?iG?v	??k?.@??G^0U#0?+?iG?v	??k?.@??G^0?U 0?0?	*?H??cd0??0+https://www.apple.com/appleca/0?+0????Reliance on this certificate by any party assumes acceptance of the then applicable standard terms and?\6?L-x?팛??w??v?w0O????=G7?@?,Ա?ؾ?s???d?yO4آ>?x?k??}9??S ?8ı??Oe statements.0
????/?Sj[d?c3w?:,V??!ںsO??6??U٧??2B???q?~?R??B$*??M?^c?K?P??????????DH?`7?uu!1??0??0??0y1
                                                                                         0	UUS10U

Apple Inc.1&0$U
               Apple Certification Authority1-0+U$Apple iPhone Certification Authorit=r ?ό?%0	+???0	1?H??
191224021p?5?=7??ܔ???
0R      1E0C0   +? 17?%?0)	*?H??
?6=nP?4.?J?ۂ?^?N3???i?B?8?Yc?&????:ü??;?
                                        !ִ??f????\???O?}?eU??B?"Я??;?n	X??o?i??31?q???2?@{??<??`?a
                                                                                                   ??S????\P??:fV*5???ρ??*?WS'{4???/Y/
??%r+G䠤?`Ϩ?\:?A???j{V?B?-??֢H????EԼk?
H?˫x(?s?"wJ?D????[?w>5??W?J?%                                                                       ----------------------------------------------------------------------------------------------------

```

#### 使用security cms -D -i XXX.mobileprovision

```
~/Desktop/sop/Spotify破解版汉化/Payload/Spotify.app » security cms -D -i embedded.mobileprovision 
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>AppIDName</key>
	<string>ftjrApp</string>
	<key>ApplicationIdentifierPrefix</key>
	<array>
	<string>Z87Y8CVBP9</string>
	</array>
	<key>CreationDate</key>
	<date>2019-12-24T02:14:57Z</date>
	<key>Platform</key>
	<array>
		<string>iOS</string>
	</array>
	<key>IsXcodeManaged</key>
	<true/>
	<key>DeveloperCertificates</key>
	<array>
		<data>MIIFsDCCBJigAwIBAgIIIW3zt8TSW08wDQYJKoZIhvcNAQEFBQAwgZYxCzAJBgNVBAYTAlVTMRMwEQYDVQQKDApBcHBsZSBJbmMuMSwwKgYDVQQLDCNBcHBsZSBXb3JsZHdpZGUgRGV2ZWxvcGVyIFJlbGF0aW9uczFEMEIGA1UEAww7QXBwbGUgV29ybGR3aWRlIERldmVsb3BlciBSZWxhdGlvbnMgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkwHhcNMTkwNjEyMDkyMDI5WhcNMjIwNjExMDkyMDI5WjCBsTEaMBgGCgmSJomT8ixkAQEMClo4N1k4Q1ZCUDkxQjBABgNVBAMMOWlQaG9uZSBEaXN0cmlidXRpb246IFRPWU9UQSBNT1RPUiBGSU5BTkNFIChDSElOQSkgQ08uLExURDETMBEGA1UECwwKWjg3WThDVkJQOTEtMCsGA1UECgwkVE9ZT1RBIE1PVE9SIEZJTkFOQ0UgKENISU5BKSBDTy4sTFREMQswCQYDVQQGEwJVUzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOoWGK9Uff9lTjIwiqgz98hRVN6CoHXJi+544FwfSPM3aok2N4Ax1Os0IMvik41DU4fBYPd/rbVJaiN02g5sD4ikwnY64jl3jXEp1eczrwYnBpjvrrfYj5pmOIaH6CAL2LvdAY/Gxfba+32YXj2RpFkrtObKUSZTKKNul3LyMrBxI5eIithog61Zl9DB+C6F1AoD+nU5jXRmILEejqX6a8a3pg3UKdSa1kaYq0w4J05YXdf5E1foyteBCL16VX4Y9ipQUi4TJ29rFGV2S85wP0JuDU5ThIbjRcKqeyNq8N4V9Qu596JEXRQmdG5+QkWvMo3QdaW+e16MoZIW3Dy+/nsCAwEAAaOCAeMwggHfMAwGA1UdEwEB/wQCMAAwHwYDVR0jBBgwFoAUiCcXCam2GGCL7Ou69kdZxVJUo7cwPwYIKwYBBQUHAQEEMzAxMC8GCCsGAQUFBzABhiNodHRwOi8vb2NzcC5hcHBsZS5jb20vb2NzcDAyLXd3ZHIwMTCCAQ8GA1UdIASCAQYwggECMIH/BgkqhkiG92NkBQEwgfEwgcMGCCsGAQUFBwICMIG2DIGzUmVsaWFuY2Ugb24gdGhpcyBjZXJ0aWZpY2F0ZSBieSBhbnkgcGFydHkgYXNzdW1lcyBhY2NlcHRhbmNlIG9mIHRoZSB0aGVuIGFwcGxpY2FibGUgc3RhbmRhcmQgdGVybXMgYW5kIGNvbmRpdGlvbnMgb2YgdXNlLCBjZXJ0aWZpY2F0ZSBwb2xpY3kgYW5kIGNlcnRpZmljYXRpb24gcHJhY3RpY2Ugc3RhdGVtZW50cy4wKQYIKwYBBQUHAgEWHWh0dHA6Ly93d3cuYXBwbGUuY29tL2FwcGxlY2EvMBYGA1UdJQEB/wQMMAoGCCsGAQUFBwMDMB0GA1UdDgQWBBSkplNmT3DMLEp0SQbep4DH3tQkVTAOBgNVHQ8BAf8EBAMCB4AwEwYKKoZIhvdjZAYBBAEB/wQCBQAwDQYJKoZIhvcNAQEFBQADggEBAFhJ5x0Kigqx7jQ9QIrwO3/ixrKexORwSbTE4KvMyEQtLIDSsWK1AgZVOFje2+jbsTAeUfgQ7SPztTdMLED/GRK5aAHQUu97FGgBtqjQfDXBh7+w3hvDMH/VDmBCBY/sphCTvm0YME5IhtgIn1XGTHkqKX2hsAPYya9DevxiP/6mTMwikpYHWhzXnv4g3PW03ZMi1EH6ry1V8kyXtauZ7XrF0zEH0IoWc+PdyTSlijb9pOArHvZ6vK5rkza82/zAmTxaZu0K+5Sc7oUslY7E5aB+X4UzVuoaJyDSJjHH5D1VNBGvOxaXmGTdiETyBWIGyZZl13DciWzSS4dFGJ61JZs=</data>
	</array>

								
	<key>Entitlements</key>
	<dict>
				
				<key>application-identifier</key>
		<string>Z87Y8CVBP9.com.tmfcn.dealerapp</string>
				
				<key>keychain-access-groups</key>
		<array>
				<string>Z87Y8CVBP9.*</string>
		</array>
				
				<key>get-task-allow</key>
		<false/>
				
				<key>com.apple.developer.team-identifier</key>
		<string>Z87Y8CVBP9</string>

	</dict>
	<key>ExpirationDate</key>
	<date>2020-12-23T02:14:57Z</date>
	<key>Name</key>
	<string>iOS Team Inhouse Provisioning Profile: com.tmfcn.dealerapp</string>
	<key>ProvisionsAllDevices</key>
	<true/>
	<key>TeamIdentifier</key>
	<array>
		<string>Z87Y8CVBP9</string>
	</array>
	<key>TeamName</key>
	<string>TOYOTA MOTOR FINANCE (CHINA) CO.,LTD</string>
	<key>TimeToLive</key>
	<integer>365</integer>
	<key>UUID</key>
	<string>fa28bc2a-7ba4-4fe4-ad89-d76eaf800821</string>
	<key>Version</key>
	<integer>1</integer>
</dict>
</plist>%                                                                                           ----------------------------------------------------------------------------------------------------
~/Desktop/sop/Spotify破解版汉化/Payload/Spotify.app »         
```


