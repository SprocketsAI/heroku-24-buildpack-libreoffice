# Heroku LibreOffice Buildpack

## Overview

This Heroku buildpack provides a simple way to install LibreOffice on Heroku dynos, enabling document conversion and processing capabilities for your applications.

## Features

- Installs LibreOffice 25.2.0
- Compatible with Heroku's Ubuntu 20.04 (Focal) stack
- Supports headless document conversion
- Minimal additional dependencies

## Requirements

- Heroku Stack: `heroku-20`
- Application using Python, Node.js, or other runtime that requires document conversion

## Installation

### Method 1: Heroku CLI

```bash
heroku buildpacks:add https://github.com/amanjain/heroku-24-buildpack-libreoffice.git
```

### Method 2: `app.json` (Heroku Button Deployments)

```json
{
  "buildpacks": [
    {
      "url": "https://github.com/amanjain/heroku-24-buildpack-libreoffice.git"
    }
  ]
}
```

### Method 3: Multiple Buildpacks

If you're using multiple buildpacks, add this as the first buildpack:

```bash
heroku buildpacks:set https://github.com/amanjain/heroku-24-buildpack-libreoffice.git
heroku buildpacks:add heroku/python  # or your primary language buildpack
```

## Usage Examples

### Python Document Conversion

```python
import subprocess

def convert_to_pdf(input_file, output_file):
    subprocess.run([
        'libreoffice', 
        '--headless', 
        '--convert-to', 'pdf', 
        '--outdir', '/tmp', 
        input_file
    ])
```

### Node.js Document Processing

```javascript
const { exec } = require('child_process');

function convertDocument(inputFile, outputPath) {
    exec(`libreoffice --headless --convert-to pdf --outdir ${outputPath} ${inputFile}`, 
    (error, stdout, stderr) => {
        if (error) {
            console.error(`Conversion error: ${error}`);
            return;
        }
        console.log('Document converted successfully');
    });
}
```

## Verification

After deployment, you can verify the LibreOffice installation:

```bash
heroku run libreoffice --headless --version
```

## Supported Formats

LibreOffice can convert between various document formats:
- Word Documents: .doc, .docx
- Spreadsheets: .xls, .xlsx
- Presentations: .ppt, .pptx
- And many more...

## Troubleshooting

- Ensure you're using the `heroku-20` stack
- Check that LibreOffice is in the PATH
- Verify document conversion requirements

### Common Issues

1. **Conversion Not Working**
   - Confirm file permissions
   - Check input file format
   - Ensure headless mode is supported

2. **Path Issues**
   ```bash
   # Verify LibreOffice location
   which libreoffice
   ```

## Performance Considerations

- Document conversion can be memory-intensive
- Consider using larger dynos for complex conversions
- Optimize conversion processes

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request


## Support

- Open an issue on GitHub
- Check [Heroku Dev Center](https://devcenter.heroku.com/) for additional resources
