## Examples

Verifiable Credentials with JSON Web Tokens:

### Example Verifiable Credential

	eyJhbGciOiJSUzI1NiJ9.eyJqdGkiOiJ1cm46dXVpZDphODdiZGZiOC1hN2RmLTRiZDktYWUwZC1kODgzMTMzNTM4ZmUiLCJzdWIiOiJkaWQ6c292OjIxdERBS0NFUmg5NXVHZ0tiSk5IWXAiLCJpc3MiOiJkaWQ6c292OjF5dlhibWdQb1VtNGRsNjZEN0toeUQiLCJuYmYiOjE1NjA3MTE0MTksImV4cCI6MTU2MDc5NzgxOSwidmMiOnsiQGNvbnRleHQiOlsiaHR0cHM6Ly93d3cudzMub3JnLzIwMTgvY3JlZGVudGlhbHMvdjEiLCJodHRwczovL3RyYWZpLmZpL2NyZWRlbnRpYWxzL3YxIl0sInR5cGUiOlsiVmVyaWZpYWJsZUNyZWRlbnRpYWwiLCJEcml2ZXJzTGljZW5zZUNyZWRlbnRpYWwiXSwiY3JlZGVudGlhbFN1YmplY3QiOnsiZHJpdmVyc0xpY2Vuc2UiOnsibGljZW5zZUNsYXNzIjoidHJ1Y2tzIn19fX0.ZdJxM0kJakKAfJI2h3Dhvf_AHUgC8-NyQRr2DMnlzCxHZyVEji0s5Sl8cNx6aPwwjDagp2KHFy6M9eONHrrpdb49AuKetyJZQUoB5aQg-sC5GMHPVyeTxzazaDWysEiCZSXc6GcirLDmHXVWMUidKDlGh8vdUtrHR0ayVbxrXQ-zKakfltaRawQ4OIRPJ-0fssojSDbRO6KnrH6MQbsl-fwTRcYnlihNIHpNTEpT68DA3QzHU1-TAX1xY3Jx2vYorihFf02mnn5d3gW4Gv2KsNbNpewIAmfL-p-abOFNmt2cLxFUSLsgaE5upDw2xAGTc3lAxvbJ7Gm4eaL8SXTQ2A

JWT Payload:

	{
		"jti": "urn:uuid:a87bdfb8-a7df-4bd9-ae0d-d883133538fe",
		"sub": "did:sov:21tDAKCERh95uGgKbJNHYp",
		"iss": "did:sov:1yvXbmgPoUm4dl66D7KhyD",
		"nbf": 1560711419,
		"exp": 1560797819,
		"vc": {
			"@context": ["https://www.w3.org/2018/credentials/v1", "https://trafi.fi/credentials/v1"],
			"type": ["VerifiableCredential", "DriversLicenseCredential"],
			"credentialSubject": {
				"driversLicense": {
					"licenseClass": "trucks"
				}
			}
		}
	}

### Example code (signing)

	VerifiableCredential verifiableCredential = new VerifiableCredential();
	verifiableCredential.getContext().add("https://trafi.fi/credentials/v1");
	verifiableCredential.getType().add("DriversLicenseCredential");
	verifiableCredential.setId("urn:uuid:a87bdfb8-a7df-4bd9-ae0d-d883133538fe");
	verifiableCredential.setIssuer("did:sov:1yvXbmgPoUm4dl66D7KhyD");
	verifiableCredential.setIssuanceDate(VerifiableCredential.DATE_FORMAT.parse("2019-06-16T18:56:59Z"));
	verifiableCredential.setExpirationDate(VerifiableCredential.DATE_FORMAT.parse("2019-06-17T18:56:59Z"));
	
	verifiableCredential.setCredentialSubject("did:sov:21tDAKCERh95uGgKbJNHYp");
	LinkedHashMap<String, Object> jsonLdCredentialSubject = verifiableCredential.getJsonLdCredentialSubject();
	LinkedHashMap<String, Object> jsonLdDriversLicense = new LinkedHashMap<String, Object> ();
	jsonLdDriversLicense.put("licenseClass", "trucks");
	jsonLdCredentialSubject.put("driversLicense", jsonLdDriversLicense);
	
	JwtVerifiableCredential jwtVerifiableCredential = JwtVerifiableCredential.fromVerifiableCredential(verifiableCredential);
	
	String jwtPayload = jwtVerifiableCredential.getPayload().toJson();
	System.out.println(jwtPayload);
	
	String jwtString = jwtVerifiableCredential.toJwt(AlgorithmIdentifiers.RSA_USING_SHA256, TestUtil.testRSAPrivateKey);
	System.out.println(jwtString);

### Example code (verifying)

	JwtVerifiableCredential jwtVerifiableCredential = JwtVerifiableCredential.fromJwt(...JWT..., AlgorithmIdentifiers.RSA_USING_SHA256, TestUtil.testRSAPublicKey);
	
	String jwtPayload = jwtVerifiableCredential.getPayload().toJson();
	String jwtPayloadVerifiableCredential = jwtVerifiableCredential.getPayloadVerifiableCredential().toJsonString();
	System.out.println(jwtPayload);
	System.out.println(jwtPayloadVerifiableCredential);
	
	VerifiableCredential verifiableCredential = jwtVerifiableCredential.toVerifiableCredential();
	System.out.println(verifiableCredential.toJsonString());
