# HomeLab - Personal File Sharing Environment

## Project Overview

This project aims to establish a home environment for storing and sharing files among household members. The initial implementation uses *Nginx* as a web server hosting a simple HTML page. Future development will enable Ubuntu users to log in to the web interface using their system credentials.

This means household members will have Ubuntu accounts with appropriate permissions and can use these same credentials to access applications through the browser via Nginx. The purpose is to create a personal "home" environment while gaining a deeper understanding of underlying technologies, rather than relying on pre-made solutions.

Feel free to share your thoughts on this project by contacting me through any available channel. Enjoy!

**Note:** Basic Linux knowledge is recommended to understand concepts discussed here. If you have questions or concerns, please reach out - I'm happy to help!

## System Specifications

This HomeLab runs on a Raspberry Pi 5 with 8GB RAM - a powerful and elegant machine. While the ARM architecture may present initial challenges, it offers excellent learning opportunities. The system uses a 500GB SSD for storage in the initial setup. (Images will be added in future updates).

<p align="center"> <img src="https://github.com/user-attachments/assets/ddc80e66-8591-472d-b69c-c6647a329137" alt="Raspberry Pi 5"> </p>

## Linux Filesystem Hierarchy Standard (FHS)

Organization is crucial for this project. During my time at 42 School, I learned the importance of structured organization and *why* certain organizational patterns exist. Following industry standards, we'll use the Linux `/opt` directory to store and access our projects.

Why this location? For reference, see page 13 of the [Linux Foundation's FHS 3.0 specification](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.pdf). The Linux root directory (`/`) contains several standard directories (each worthy of further study), including our target location `/opt`:

```shell
markos@ubuntu:/$ ls
bin   dev  home  lost+found  mnt  proc  run   snap  sys  usr
boot  etc  lib   media       opt  root  sbin  srv   tmp  var
```

The `/opt` directory will serve as our project hub. Further details about the folder structure for each tool will be available in this README or in dedicated folders within this repository (to be added later).

## NGINX Configuration

The current setup is in its early stages. The immediate goal is to deploy Nginx using Docker containers, running as PID 1. Once the HTML page is accessible within the HomeLab network, this section will be updated with further details.

### Structure

```
/opt/production/
├── nginx/                    # Nginx web server
│   ├── conf/                 # Configuration files
│   │   ├── nginx.conf        # Main configuration
│   │   └── sites-enabled/    # Virtual host configurations
│   │       └── default.conf  # Default site configuration
│   ├── html/                 # Web content
│   │   ├── index.html        # Homepage
│   │   ├── css/              # Stylesheets
│   │   └── js/               # JavaScript files
│   └── logs/                 # Log files
│
└── docker-compose.yml        # Docker Compose configuration
```

## Current Status

- Setting up Nginx with Docker
- Preparing initial web interface
- Planning user authentication integration

## Next Steps

- Establish secure user authentication
- Implement file sharing capabilities
- Set up proper access controls
- Develop a user-friendly interface
