# Odoo 17 Setup in Google Colab

![image](https://github.com/user-attachments/assets/21cdc8e0-551c-4d2d-a1ee-a9ec22fe9333)

This repository contains scripts to set up and run Odoo 17 in Google Colab with PostgreSQL and ngrok for public access.

## Prerequisites

- Google Colab account
- ngrok authentication token (obtain from [ngrok dashboard](https://dashboard.ngrok.com/auth/your-authtoken))

## System Components

- PostgreSQL database
- Python 3 virtual environment
- Odoo 17.0 from official repository
- ngrok for public access

## Installation Steps

### 1. System Updates and PostgreSQL Setup

```bash
# Update system and install PostgreSQL
apt-get update
apt-get install -y postgresql postgresql-client
service postgresql start

# Configure PostgreSQL user and database
sudo -u postgres psql -c "DROP DATABASE IF EXISTS odoo;"
sudo -u postgres psql -c "DROP USER IF EXISTS odoo;"
sudo -u postgres psql -c "CREATE USER odoo WITH PASSWORD 'odoo' CREATEDB;"
sudo -u postgres psql -c "CREATE DATABASE odoo OWNER odoo;"
```

### 2. System Dependencies

The following system dependencies are installed:
- Python development tools
- Build essentials
- XML and image processing libraries
- PostgreSQL development files
- wkhtmltopdf for PDF generation

### 3. Odoo Installation

```bash
# Clone Odoo repository
git clone https://github.com/odoo/odoo.git --depth 1 --branch 17.0 --single-branch src/odoo17c

# Create and activate virtual environment
python3 -m venv .venv
source .venv/bin/activate
```

### 4. Python Dependencies

Key Python packages installed:
- wheel
- psycopg2-binary
- Babel 2.13.1
- lxml_html_clean
- Additional requirements from Odoo's requirements.txt

### 5. Configuration

The Odoo configuration file is created at `conf/odoo.conf` with the following settings:
- Admin password: master
- Database host: localhost
- Database port: 5432
- Database user: odoo
- Database password: odoo
- XML-RPC port: 8070

### 6. Database Initialization

```bash
python3 src/odoo17c/odoo-bin -c conf/odoo.conf -d odoo -i base --stop-after-init
```

### 7. Public Access Setup

```python
# Install and configure ngrok
pip install pyngrok
ngrok authtoken YOUR_NGROK_AUTH_TOKEN

# Create tunnel and start server
from pyngrok import ngrok
public_url = ngrok.connect(8070)
python3 src/odoo17c/odoo-bin -c conf/odoo.conf
```

## Usage

1. Run all cells in the Colab notebook sequentially
2. Wait for the ngrok URL to be displayed
3. Access Odoo through the provided ngrok URL
4. Login credentials:
   - Database: odoo
   - Email: admin
   - Password: admin

## Notes

- The setup is intended for development and testing purposes
- Data persistence is limited to the Colab session duration
- For production use, additional security measures should be implemented
- The ngrok URL changes with each new session

## Troubleshooting

If you encounter issues:
1. Ensure all cells are executed in order
2. Check that PostgreSQL service is running
3. Verify ngrok authentication token is correct
4. Confirm all required ports are available (8070 for Odoo)

## License

This setup follows Odoo's LGPL license. Refer to [Odoo's license](https://github.com/odoo/odoo/blob/17.0/LICENSE) for more details.
