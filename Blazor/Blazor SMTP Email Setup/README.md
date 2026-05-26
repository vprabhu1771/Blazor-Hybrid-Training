For sending SMTP emails in a Blazor app, the recommended approach is:

* **Blazor Server** → send directly using SMTP
* **Blazor WebAssembly** → use an API/backend because SMTP credentials cannot be exposed in the browser
* **Blazor Hybrid / .NET MAUI Blazor** → can send directly like a normal .NET app

The best modern library is [MailKit](https://github.com/jstedfast/MailKit?utm_source=chatgpt.com)

Install package:

```bash
dotnet add package MailKit
```

---

# SMTP Email Service Example

## EmailService.cs

```csharp
using MailKit.Net.Smtp;
using MimeKit;

namespace MauiApp.Services
{
    public class EmailService
    {
        public async Task SendEmailAsync(
            string toEmail,
            string subject,
            string body)
        {
            var email = new MimeMessage();

            email.From.Add(
                new MailboxAddress(
                    "Maui App",
                    "yourgmail@gmail.com"));

            email.To.Add(
                MailboxAddress.Parse(toEmail));

            email.Subject = subject;

            email.Body = new TextPart("html")
            {
                Text = body
            };

            using var smtp = new SmtpClient();

            await smtp.ConnectAsync(
                "smtp.gmail.com",
                587,
                MailKit.Security.SecureSocketOptions.StartTls);

            await smtp.AuthenticateAsync(
                "yourgmail@gmail.com",
                "YOUR_APP_PASSWORD");

            await smtp.SendAsync(email);

            await smtp.DisconnectAsync(true);
        }
    }
}
```

---

# Register Service

## MauiProgram.cs

```csharp
builder.Services.AddSingleton<EmailService>();
```

---

# Use in Razor Page

## SendMail.razor

```razor
@page "/sendmail"

@inject EmailService EmailService

<h3>Send Email</h3>

<input @bind="toEmail" placeholder="To Email" class="form-control mb-2" />

<input @bind="subject" placeholder="Subject" class="form-control mb-2" />

<textarea @bind="message"
          class="form-control mb-2">
</textarea>

<button class="btn btn-primary"
        @onclick="SendEmail">
    Send
</button>

<p>@status</p>

@code {

    string toEmail = "";
    string subject = "";
    string message = "";
    string status = "";

    private async Task SendEmail()
    {
        try
        {
            await EmailService.SendEmailAsync(
                toEmail,
                subject,
                message);

            status = "Email Sent";
        }
        catch (Exception ex)
        {
            status = ex.Message;
        }
    }
}
```

---

# Gmail SMTP Settings

| Setting   | Value          |
| --------- | -------------- |
| SMTP Host | smtp.gmail.com |
| Port      | 587            |
| Security  | STARTTLS       |

---

# Important for Gmail

You must use:

* Gmail with **2-Step Verification**
* Generate **App Password**

Google Account → Security → App Passwords

Do NOT use your normal Gmail password.

---

# Example HTML Email

```csharp
string body = @"
<h2>Welcome</h2>
<p>Your account created successfully.</p>";
```

---

# Recommended Production Setup

Store SMTP credentials in:

## appsettings.json

```json
{
  "Smtp": {
    "Host": "smtp.gmail.com",
    "Port": 587,
    "Username": "yourgmail@gmail.com",
    "Password": "app-password"
  }
}
```

Instead of hardcoding credentials in code.

---

Alternative SMTP providers:

* [SendGrid](https://sendgrid.com?utm_source=chatgpt.com)
* [Mailtrap](https://mailtrap.io?utm_source=chatgpt.com)
* [Brevo](https://www.brevo.com?utm_source=chatgpt.com)
* [Amazon SES](https://aws.amazon.com/ses/?utm_source=chatgpt.com)
