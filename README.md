using System;
using System.Net.Mail;
using System.Text;

public class AccountRecovery
{
    // Placeholder for database access or user data storage
    private static Dictionary<string, string> users = new Dictionary<string, string>()
    {
        {"john.doe@example.com", "password123"}, 
        {"jane.smith@example.com", "securepass"}
    };

    public static void Main(string[] args)
    {
        Console.WriteLine("Welcome to Account Recovery");
        Console.WriteLine("Please enter your email address:");
        string email = Console.ReadLine();

        if (users.ContainsKey(email))
        {
            // Send recovery email with a temporary password
            string tempPassword = GenerateTemporaryPassword();
            SendRecoveryEmail(email, tempPassword);
            Console.WriteLine("A temporary password has been sent to your email address.");
        }
        else
        {
            Console.WriteLine("Email address not found. Please try again.");
        }

        Console.ReadKey();
    }

    private static string GenerateTemporaryPassword()
    {
        // Generate a random temporary password (replace with a more secure implementation)
        Random random = new Random();
        const string chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        return new string(Enumerable.Repeat(chars, 8)
            .Select(s => s[random.Next(s.Length)]).ToArray());
    }

    private static void SendRecoveryEmail(string email, string tempPassword)
    {
        // Replace with your actual email credentials
        string senderEmail = "your_email@example.com";
        string senderPassword = "your_email_password";

        // Create a mail message
        MailMessage message = new MailMessage();
        message.From = new MailAddress(senderEmail);
        message.To.Add(email);
        message.Subject = "Account Recovery";
        message.Body = $"Your temporary password is: {tempPassword}nnPlease login and change your password.";

        // Create an SMTP client
        SmtpClient client = new SmtpClient("smtp.example.com", 587); // Replace with your SMTP server and port
        client.EnableSsl = true;
        client.Credentials = new NetworkCredential(senderEmail, senderPassword);

        try
        {
            client.Send(message);
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error sending email: " + ex.Message);
        }
    }
}
