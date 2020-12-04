# Providers

|Formatter|Method                       |Comment|V2        |V3        |
|---------|-----------------------------|-------|----------|----------|
|medical  |bloodType                    |       |          |          |
|         |bloodRh                      |       |          |          |
|         |bloodGroup                   |       |          |          |
|person   |title                        |       |          |          |
|         |titleMale                    |       |deprecated|@title    |
|         |titleFemale                  |       |deprecated|@title    |
|         |name                         |       |          |          |
|         |firstName                    |       |          |          |
|         |firstNameMale                |       |deprecated|@firstName|
|         |firstNameFemale              |       |deprecated|@firstName|
|         |lastName                     |       |          |          |
|address  |cityPrefix                   |       |          |          |
|         |secundaryAddress             |       |          |          |
|         |state                        |       |          |          |
|         |stateAbbr                    |       |          |          |
|         |citySuffix                   |       |          |          |
|         |streetSuffix                 |       |          |          |
|         |buildingNumber               |       |          |          |
|         |city                         |       |          |          |
|         |streetName                   |       |          |          |
|         |streetAddress                |       |          |          |
|         |postcode                     |       |          |          |
|         |address                      |       |          |          |
|         |country                      |       |          |          |
|         |latitude                     |       |          |          |
|         |longitude                    |       |          |          |
|         |localCoordinates             |       |          |          |
|phonenumber|phoneNumber                  |       |          |          |
|         |e164PhoneNumber              |       |          |          |
|         |emei                         |       |          |          |
|company  |company                      |       |          |          |
|         |companySuffix                |       |          |          |
|         |jobTitle                     |       |          |          |
|text     |realText                     |       |          |          |
|html     |randomHtml                   |       |          |          |
|image    |imageUrl                     |       |remove    |          |
|         |image                        |       |remove    |          |
|file     |mimeType                     |       |          |          |
|         |fileExtension                |       |          |          |
|         |file                         |       |          |          |
|miscellaneous|boolean                      |       |          |          |
|         |md5                          |       |          |          |
|         |sha1                         |       |          |          |
|         |sha256                       |       |          |          |
|         |locale                       |       |          |          |
|         |countryCode                  |       |          |          |
|         |countryISOAlpha3             |       |          |          |
|         |languageCode                 |       |          |          |
|         |currencyCode                 |       |          |          |
|         |emoji                        |       |          |          |
|biased   |biasedNumberBetween          |       |          |          |
|barcode  |ean13                        |       |          |          |
|         |ean8                         |       |          |          |
|         |isbn10                       |       |          |          |
|         |isbn13                       |       |          |          |
|uuid     |uuid                         |       |          |          |
|color    |hexColor                     |       |          |          |
|         |safeHexColor                 |       |          |          |
|         |rgbColorAsArray              |       |          |          |
|         |rgbColor                     |       |          |          |
|         |rgbCssColor                  |       |          |          |
|         |rgbaCssColor                 |       |          |          |
|         |safeColorName                |       |          |          |
|         |colorName                    |       |          |          |
|         |hslColor                     |       |          |          |
|         |hslColorAsArray              |       |          |          |
|money    |creditCardType               |       |          |          |
|         |creditCardNumber             |       |          |          |
|         |creditCardExpirationDate     |       |          |          |
|         |creditCardExpirationDateString|       |          |          |
|         |creditCardDetails            |       |          |          |
|         |iban                         |       |          |          |
|         |swiftBicNumber               |       |          |          |
|useragent|userAgent                    |       |          |          |
|         |macProcessor                 |       |          |          |
|         |linuxProcessor               |       |          |          |
|         |chrome                       |       |          |          |
|         |firefox                      |       |          |          |
|         |safari                       |       |          |          |
|         |opera                        |       |          |          |
|         |internetExplorer             |       |          |          |
|         |edge                         |new!   |          |          |
|         |windowsPlatformToken         |       |          |          |
|         |macPlatformToken             |       |          |          |
|         |linuxPlatformToken           |       |          |          |
|internet |email                        |       |          |          |
|         |safeEmail                    |       |          |          |
|         |freeEmail                    |       |          |          |
|         |companyEmail                 |       |          |          |
|         |freeEmailDomain              |       |          |          |
|         |safeEmailDomain              |       |          |          |
|         |userName                     |       |          |          |
|         |password                     |       |          |          |
|         |domainName                   |       |          |          |
|         |domainWord                   |       |          |          |
|         |tld                          |       |          |          |
|         |url                          |       |          |          |
|         |slug                         |       |          |          |
|         |ipv4                         |       |          |          |
|         |localIpv4                    |       |          |          |
|         |ipv6                         |       |          |          |
|         |macAddress                   |       |          |          |
|datetime |unixTime                     |       |          |          |
|         |dateTime                     |       |          |          |
|         |dateTimeAD                   |       |          |          |
|         |iso8601                      |       |          |          |
|         |date                         |       |          |          |
|         |time                         |       |          |          |
|         |dateTimeBetween              |       |          |          |
|         |dateTimeInInterval           |       |          |          |
|         |dateTimeThisCentury          |       |          |          |
|         |dateTimeThisDecade           |       |          |          |
|         |dateTimeThisYear             |       |          |          |
|         |dateTimeThisMonth            |       |          |          |
|         |amPm                         |       |          |          |
|         |dayOfMonth                   |       |          |          |
|         |dayOfWeek                    |       |          |          |
|         |month                        |       |          |          |
|         |monthName                    |       |          |          |
|         |year                         |       |          |          |
|         |century                      |       |          |          |
|         |timezone                     |       |          |          |
|lorum    |word                         |       |          |          |
|         |words                        |       |          |          |
|         |sentence                     |       |          |          |
|         |sentences                    |       |          |          |
|         |paragraph                    |       |          |          |
|         |paragraphs                   |       |          |          |
|         |text                         |       |          |          |
|number   |randomDigit                  |       |          |          |
|         |randomDigitNot               |       |          |          |
|         |randomDigitNotNull           |       |          |          |
|         |randomNumber                 |       |          |          |
|         |randomFloat                  |       |          |          |
|         |numberBetween                |       |          |          |
|text     |realText                     |       |          |          |
|base     |randomLetter                 |       |          |          |
|         |randomElements               |       |          |          |
|         |randomElement                |       |          |          |
|         |shuffle                      |       |          |          |
|         |numerify                     |       |          |          |
|         |lexify                       |       |          |          |
|         |bothify                      |       |          |          |
|         |asciify                      |       |          |          |
|         |passthrough                  |       |          |          |
|         |randomAscii                  |       |          |          |
|         |randomKey                    |       |          |          |
|         |shuffleArray                 |       |          |          |
|         |shuffleString                |       |          |          |
|         |regexify                     |       |          |          |
|         |toLower                      |       |          |          |
|         |toUpper                      |       |          |          |
|         |optional                     |       |          |          |
|         |unique                       |       |          |          |
|         |valid                        |       |          |          |
