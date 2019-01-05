# GygReader

A tool for suppliers on the the GetYourGuide platform to retrieve booking details.
Ideal to automize the processing of GetYourGuide bookings.

## What it does
Unfortunatly GetYourGuide does not provide webhooks or an API for suppliers to retrieve details from bookings. This can be a problem if GetYourGuide orders need to be processed automatically or registered in your own application/backend.

However an email is sent to the supplier for each booking with the GetYourGuide booking number in the subject. Using services like Zapier or inbound email parsing of Mailgun or SendGrid a webhook can be send to an endpoint retrieving the subject of the GetYourGuide booking email.
After parsing the GetYourGuide order number from the subject the **GygReader** can be used to retrieve the order details and further process the information.
For example in Django retrieving the GetYourGuide order number from a SendGrid webhook can look like this:

```
def endpoint_for_webhook(request):
  subject = request.data["subject"]
  re.search('(GYG)(\d+)',subject).group(0)
```

## How to install and use
To install run:
```
pip install gyg_reader
```

Example use:
```
from gyg_reader import GygReader


gygreader = GygReader("your@email.com", "your_password")
booking = gygreader.get_booking("GYG11111111")
```
Where GYG11111111 is a GetYourGuide booking number, which can be retrieved like in the example above from the email subject.

`booking` contains following dictionary:
```
{'customer_email': 'die@hard.com',
 'customer_name': 'John Mcclane',
 'customer_phone': '+1 202-456-1111',
 'Tour in NYC',
 'amount': '1'}
```

The full response containing the full booking data can be access following:
```
gygreader.response.json()
```

## Requirements
* requests
* lxml

