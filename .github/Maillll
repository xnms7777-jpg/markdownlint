import os
import email
from email.header import decode_header
import imaplib
from dotenv import load_dotenv

load_dotenv()

EMAIL_ACCOUNT = os.getenv("GMAIL_EMAIL")
EMAIL_PASSWORD = os.getenv("GMAIL_APP_PASSWORD")
IMAP_SERVER = "imap.gmail.com"


def connect_imap(custom_email=None, custom_password=None):
    account = custom_email or EMAIL_ACCOUNT
    password = custom_password or EMAIL_PASSWORD
    
    try:
        mail = imaplib.IMAP4_SSL(IMAP_SERVER)
        mail.login(account, password)
        print("Connected successfully.")
        return mail
    except imaplib.IMAP4.error as e:
        print(f"Connection failed: {e}")
        return None
    except Exception as e:
        print(f"Unexpected error: {e}")
        return None


def decode_email_header(value):
    if not value:
        return "(No Subject)"

    try:
        decoded_parts = []
        for part, encoding in decode_header(value):
            if isinstance(part, bytes):
                decoded_parts.append(part.decode(encoding or "utf-8", errors="ignore"))
            else:
                decoded_parts.append(part)
        return "".join(decoded_parts)
    except Exception:
        return str(value)


def get_email_body(msg):
    if msg.is_multipart():
        for part in msg.walk():
            content_type = part.get_content_type()
            content_disposition = str(part.get("Content-Disposition"))

            if content_type == "text/plain" and "attachment" not in content_disposition:
                payload = part.get_payload(decode=True)
                if payload:
                    charset = part.get_content_charset() or "utf-8"
                    return payload.decode(charset, errors="ignore")

            if content_type == "text/html" and "attachment" not in content_disposition:
                payload = part.get_payload(decode=True)
                if payload:
                    charset = part.get_content_charset() or "utf-8"
                    return payload.decode(charset, errors="ignore")
    else:
        payload = msg.get_payload(decode=True)
        if payload:
            charset = msg.get_content_charset() or "utf-8"
            return payload.decode(charset, errors="ignore")

    return "(No Text)"
