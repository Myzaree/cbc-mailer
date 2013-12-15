cbc-mailer
==========

Simple mailer interface with a minimalist design.

About
=====

Provides handing for sending email using multiple interfaces. The package currently has the PEAR mailer wrapper included, but can include any number of interfaces for ease of use through a minimalistic API.

How to Use
==========

Deafult use requires the PEAR::Mail package.

Create the base configs for the email, including any headers and the preferred backend interface.

```php
use Mailer\Email\Mailer;
use Mailer\Email\PearBackend;

$headers = array(
	'To' => 'recipient@host.com',
	'From' => 'sender@host.com',
	'Bcc' => 'bcc.recipient@host.com',
	'Subject' => 'Test Email From ' . $_SERVER['SERVER_NAME']
);

$mailer = new Mailer(new PearBackend());
```

Set the templates, add attachments, and send the email.

```php
$data = array('one', 'two', 'three');

$dir = dirname(__FILE__);
$template_html = $dir . '/templates/template_html.php';
$template_text = $dir . '/templates/template_text.php';
$attachment_pdf = $dir . '/attachments/test.pdf';
$attachment_png = $dir . '/attachments/test.png';

$mailer = new Mailer(new PearBackend());
$mailer->setHeaders($headers);
$mailer->setHtmlTemplate($template_html, $data);
$mailer->setTextTemplate($template_text, $data);
$mailer->addAttachment($attachment_pdf, 'test.pdf');
$mailer->addHtmlImage($attachment_png, 'test_png');
$mailer->sendEmail();
```

Email template handling using pure HTML, CSS, and PHP. Attached will be test.pdf from the Mailer. The $data variable comes from the template handling in the Mailer.

```html
<html>
<head>
  	<title>Test Email</title>
  	<style>
  	p {
  		padding: 1em;
  		margin: 1em;
  	}

  	img {
  		height: 100px;
  		width: 100px;
  	}
  	</style>
</head>
<body>
<img src="test_png" alt="Test Image" />
<p>
    Custom Data:
    <?php var_dump($data) ?>
</p>
</body>
</html>
```
