# Disable Jenkins Security

#### Steps to Disable Jenkins Security via `sed`:

1.  **Stop Jenkins**:

    ```bash
    sudo systemctl stop jenkins
    ```
2.  **Run the `sed` command**:

    ```bash
    sudo sed -i 's/<useSecurity>true<\/useSecurity>/<useSecurity>false<\/useSecurity>/g' /var/lib/jenkins/config.xml
    ```
3.  **Optional: Backup the config first**:

    ```bash
    sudo cp /var/lib/jenkins/config.xml /var/lib/jenkins/config.xml.bak
    ```
4.  **Restart Jenkins**:

    ```bash
    sudo systemctl start jenkins
    ```
5. **Now Jenkins will start with NO authentication** (fully open).
