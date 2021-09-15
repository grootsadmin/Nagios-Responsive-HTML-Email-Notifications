# Gmetrics Responsive HTML Email Notifications Templates

Here are two php built scripts, one for sending Host Notifications and one for sending Service Notifications, as Responsive HTML email messages from your Nagios server.

**Copyrights**  
Copyright (&#169;) 2021 Groots Software <support@groots.in>

## Screenshots
### Host Alert Notification
![nagios_host_mail screenshot](https://github.com/heiniha/Nagios-Responsive-HTML-Email-Notifications/blob/GitHub/image-examples/host_html_email.png?raw=true)
### Service Alert Notification
![nagios_service_mail screenshot](https://github.com/heiniha/Nagios-Responsive-HTML-Email-Notifications/blob/GitHub/image-examples/service_html_email.png?raw=true)

## Installation
Copy the php-html-email folder to the Nagios plugins directory (usually named 'libexec') and make sure all files are owned by the user nagios uses and also that all files are made executable by the nagios user!  
Then configure Nagios.

## Gmetrics Configuration
* For the host and services notification alerts, add these lines to your Nagios commands file (usually located in the Nagios configurations directory and named 'objects') as command definitions:

```
# 'notify-host-by-email-html' command definition
define command {
	command_name	notify-host-by-email-html
	command_line    $USER1$/php-html-email/gmetrics_host_email "$NOTIFICATIONTYPE$" "$HOSTNAME$" "$HOSTALIAS$" "$HOSTSTATE$" "$HOSTADDRESS$" "$HOSTOUTPUT$" "$SHORTDATETIME$" "$CONTACTEMAIL$" "$TOTALHOSTSUP$" "$TOTALHOSTSDOWN$" "$NOTIFICATIONAUTHOR$" "$NOTIFICATIONCOMMENT$" "$LONGDATETIME$" "$HOSTDURATION$" "$HOSTDURATIONSEC$" "$LASTHOSTCHECK$" "$LASTHOSTSTATECHANGE$" "$NOTIFICATIONISESCALATED$" "$HOSTATTEMPT$" "$MAXHOSTATTEMPTS$" "$NOTIFICATIONRECIPIENTS$"
}

# 'notify-service-by-email-html' command definition
define command {
	command_name	notify-service-by-email-html
	command_line	$USER1$/php-html-email/gmetrics_service_email "$NOTIFICATIONTYPE$" "$HOSTNAME$" "$HOSTALIAS$" "$HOSTSTATE$" "$HOSTADDRESS$" "$SERVICEOUTPUT$" "$SHORTDATETIME$" "$SERVICEDESC$" "$SERVICESTATE$" "$CONTACTEMAIL$" "$SERVICEDURATIONSEC$" "$SERVICEEXECUTIONTIME$" "$TOTALSERVICESWARNING$" "$TOTALSERVICESCRITICAL$" "$TOTALSERVICESUNKNOWN$" "$LASTSERVICEOK$" "$LASTSERVICEWARNING$" "$SERVICENOTIFICATIONNUMBER$" "$LONGSERVICEOUTPUT$" "$NOTIFICATIONAUTHOR$" "$NOTIFICATIONCOMMENT$" "$SERVICEDURATION$" "$NOTIFICATIONISESCALATED$" "$SERVICEATTEMPT$" "$MAXSERVICEATTEMPTS$" "$NOTIFICATIONRECIPIENTS$"
}
```

* Now that the command object has been configured, you must tell your contacts to use this new command. I have a generic-contact template that all of my individual contacts inherit from, thus I only have to update the template to use the new command, like seen here below:

```
define contact{
	name	generic-contact
	register	0
	service_notification_commands	notify-service-by-email-html
	host_notification_commands	notify-host-by-email-html
```

* Restart Nagios to pick up the changes.

## PHPMailer Configuration

Open up these files and and scroll to the bottom to find the default configuration using PHPMailer. Say, to make it send email to your Gmail via Google SMTP server, just follow the configuration as stated.
```
<repo>/php-html-email/gmetrics_host_email
<repo>/php-html-email/gmetrics_service_email
```

### Using Gmail SMTP

```
$mail = new PHPMailer;
#$mail->SMTPDebug = 3;
$mail->isSMTP();
$mail->SMTPAuth = true;
$mail->SMTPSecure = 'tls';
$mail->Host = 'smtp.gmail.com';
$mail->Port = 587;
$mail->Username = 'someone@example.com';
$mail->Password = 'somepassword';
```
