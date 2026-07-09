# SRPAL AMC Portal

A professional Flask web application for managing AMC (Annual Maintenance Contract) checklist uploads for SRPAL (South-Eastern Railway - Post and Level crossing).

## Features

✅ **Responsive Web Upload Page**
- Engineer information capture
- Railway hierarchy selection (Zone → Division → Station)
- AMC month selection
- File upload with multiple format support
- Remarks/comments field

✅ **Dynamic Station Selection**
- Load station master from Excel file
- Auto-populate divisions based on selected zone
- Auto-populate stations based on selected division
- Full cascading dropdown functionality

✅ **Smart File Management**
- Support for multiple file formats: PDF, ZIP, JPG, PNG
- Large file support up to 200 MB
- Organized folder structure based on hierarchy
- Automatic file naming with timestamps

✅ **Database Tracking**
- SQLite database for upload records
- Comprehensive tracking of all uploads
- Search and filter capabilities
- Performance indexes for fast queries

✅ **Admin Dashboard**
- Key statistics (Total Stations, Uploaded Count)
- Month-wise upload analysis
- Zone and Division filters
- Detailed upload history table
- Pagination for large datasets

✅ **Export Functionality**
- Download upload status as Excel file
- Automatic formatting and column sizing
- Ready for reporting and analysis

✅ **Bootstrap UI**
- Fully responsive design
- Mobile-friendly interface
- Modern and professional appearance
- Dark/Light themes support

## Project Structure

```
SRPAL AMC Portal/
├── app.py                 # Main Flask application
├── database.py            # Database initialization and operations
├── setup.py               # Setup script to create sample data
├── requirements.txt       # Python dependencies
├── templates/
│   ├── index.html        # Upload form page
│   └── dashboard.html    # Admin dashboard
├── static/
│   └── style.css         # Custom CSS styling
├── data/
│   └── StationMaster.xlsx # Station master data (created by setup.py)
└── uploads/              # File storage (auto-created)
```

## Installation

### Prerequisites
- Python 3.8 or higher
- pip (Python package installer)
- Windows, macOS, or Linux

### Step 1: Clone/Download the Project
```bash
cd "d:\AMC checklist\New folder"
```

### Step 2: Create Virtual Environment (Recommended)
```bash
# Windows
py -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
# Windows (using Python launcher)
py -m pip install -r requirements.txt

# macOS/Linux
pip install -r requirements.txt
```

### Step 4: Create Sample Data
```bash
py setup.py
```

This creates the `StationMaster.xlsx` file with sample station data for:
- **Zones**: ECOR, ER, SR, SWR
- **Divisions**: Waltair, Sambalpur, Adra, Kharagpur, Chennai Central, Bengaluru
- **Stations**: 35+ sample stations

### Step 5: Run the Application
```bash
py app.py
```

The application will start on `http://localhost:5000`

## Usage

### Upload Page (`/`)
1. Enter Engineer Name (required)
2. Enter Email ID (optional but recommended)
3. Select Zonal Railway from dropdown
4. Select Division (auto-populated based on zone)
5. Select Station (auto-populated based on division)
6. Select AMC Month
7. Add optional remarks
8. Choose file to upload (PDF, ZIP, JPG, PNG up to 200 MB)
9. Click "Submit Upload"

### Dashboard (`/dashboard`)
1. View key statistics
2. Apply filters by Zone, Division, or Month
3. Browse upload history with pagination
4. Export upload status to Excel

## File Organization

Uploaded files are automatically organized as follows:

```
uploads/
└── {Zonal Railway}/
    └── {Division}/
        └── {YYYY}_{Month}/
            └── {Station}/
                └── {timestamp}_{filename}
```

**Example**:
```
uploads/
└── ECOR/
    └── Waltair/
        └── 2026_July/
            └── Komatipalli/
                └── 20260708_153045_checklist.pdf
```

## Database Schema

### amc_uploads Table

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary Key, Auto-increment |
| engineer_name | TEXT | Name of the engineer uploading |
| email | TEXT | Email address |
| zone | TEXT | Zonal Railway |
| division | TEXT | Division |
| station | TEXT | Station Name |
| month | TEXT | AMC Month (YYYY_Month format) |
| file_path | TEXT | Path to uploaded file |
| original_filename | TEXT | Original file name |
| file_size | INTEGER | File size in bytes |
| upload_date | TIMESTAMP | Upload date and time |
| remarks | TEXT | Additional remarks |
| status | TEXT | Upload status (default: 'completed') |

**Indexes**: Created on zone, division, month, and station for fast queries

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Upload page |
| `/dashboard` | GET | Admin dashboard |
| `/upload` | POST | Handle file upload |
| `/api/divisions/<zone>` | GET | Get divisions for a zone |
| `/api/stations/<zone>/<division>` | GET | Get stations for zone and division |
| `/api/uploads` | GET | Get all uploads (with optional filters) |
| `/api/export-excel` | GET | Export upload status as Excel |

## Configuration

Edit `app.py` to modify:

```python
# Maximum file size (default: 200 MB)
app.config['MAX_CONTENT_LENGTH'] = 200 * 1024 * 1024

# Allowed file extensions
ALLOWED_EXTENSIONS = {'pdf', 'zip', 'jpg', 'jpeg', 'png'}

# Upload folder location
app.config['UPLOAD_FOLDER'] = 'uploads'
```

## Customization

### Add More Stations
1. Edit `setup.py` with new station data
2. Run `python setup.py` to regenerate StationMaster.xlsx
3. Restart the application

### Modify UI
- Edit `templates/index.html` and `templates/dashboard.html` for HTML structure
- Edit `static/style.css` for styling

### Add More AMC Months
- Edit `get_amc_months()` function in `app.py`

## Troubleshooting

### Port Already in Use
```bash
# Change port in app.py
if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5001)  # Changed to 5001
```

### File Upload Fails
- Check file size (max 200 MB)
- Verify file format is in allowed list (PDF, ZIP, JPG, PNG)
- Ensure `uploads` folder has write permissions

### StationMaster.xlsx Not Found
```bash
python setup.py
```

### Database Lock Error
- Close all instances of the application
- Delete `amc_portal.db` if corrupted
- Restart the application (database will be recreated)

## Performance

- Supports up to 200 MB file uploads
- Indexed database queries for fast filtering
- Pagination in dashboard (10 items per page)
- Optimized CSS and Bootstrap for responsive design

## Security Notes

⚠️ **For Production Use**:
1. Set `debug=False` in app.py
2. Use a production WSGI server (Gunicorn, uWSGI)
3. Set up HTTPS/SSL certificates
4. Implement user authentication
5. Add file scanning/validation
6. Set up proper backup strategy
7. Use environment variables for configuration

## Testing

### Manual Testing Checklist
- [ ] Upload a PDF file
- [ ] Upload a ZIP file
- [ ] Test zone/division/station cascading
- [ ] Verify folder structure in uploads/
- [ ] Check database records
- [ ] Test filters in dashboard
- [ ] Export Excel file
- [ ] Test on mobile device
- [ ] Test file upload > 100 MB
- [ ] Test invalid file format

## Maintenance

### Regular Tasks
- Monitor upload folder disk usage
- Archive old uploads periodically
- Backup database file (`amc_portal.db`)
- Review error logs
- Clean up temporary files

### Database Maintenance
```bash
# View database (Windows - Python launcher)
py -c "import database; uploads = database.get_all_uploads(); print(uploads)"

# Get statistics (Windows - Python launcher)
py -c "import database; stats = database.get_statistics(); print(stats)"
```

## Dependencies

- **Flask** (2.3.3): Web framework
- **Werkzeug** (2.3.7): WSGI utility library
- **pandas** (2.0.3): Data manipulation
- **openpyxl** (3.1.2): Excel file creation

## Support

For issues or questions:
1. Check troubleshooting section
2. Review application logs
3. Verify all dependencies are installed
4. Ensure proper folder permissions

---

## Azure App Service Deployment

This application is ready for deployment to Azure App Service.

### Quick Deployment Steps

1. **Push to GitHub**: Commit code to GitHub repository
2. **Create Azure App Service**: Set up Linux App Service with Python 3.9+
3. **Configure Deployment**: Connect GitHub repository via Deployment Center
4. **Set Environment Variables**: Configure in Azure Portal Application Settings
5. **Deploy**: Push to main branch, Azure auto-deploys

### For Complete Azure Setup

See [AZURE_DEPLOYMENT.md](AZURE_DEPLOYMENT.md) for:
- Step-by-step Azure setup
- GitHub integration
- Configuration details
- Troubleshooting
- Performance optimization
- Monitoring & logging

### What's Included for Azure

- ✅ `wsgi.py` - WSGI entry point for production
- ✅ `gunicorn` in requirements.txt - Production WSGI server
- ✅ `config.py` - Environment-based configuration
- ✅ `startup.sh` - Azure startup script
- ✅ `.gitignore` - For GitHub deployment
- ✅ `AZURE_DEPLOYMENT.md` - Complete deployment guide

### Local vs Azure

**Local Development** (Windows):
```powershell
py -m pip install -r requirements.txt
py app.py
```
Runs with Flask development server, debug mode enabled.

**Azure Production**:
```
gunicorn --workers 4 --worker-class sync --bind 0.0.0.0:8000 wsgi:app
```
Runs with gunicorn WSGI server, debug mode disabled.

---

## License

This application is designed for SRPAL (South-Eastern Railway) internal use.

## Version

**v1.0.0** - Initial Release (July 2026)

---

**Last Updated**: July 8, 2026
