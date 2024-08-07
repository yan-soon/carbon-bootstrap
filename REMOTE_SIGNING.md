# Remote Signing

The following instructions is **ONLY** necessary if you are running a validator node **AND** using a remote signing service (e.g. horcrux)

**:warning: Run these steps in sequence, do not skip any steps! :warning:**

## 1. Add the following configs to your `config.toml`, in your **VALIDATOR NODE**

  * #### i) Run the following command to add the configs.

    ```bash
    CARBON_HOME_PATH=~/.carbon # Update home path if necessary
    sed -i '/\[oraclesvc\]/a\
    \
    # Enables sub account signing for votes\
    enable_sub_account_signing = true\
    \
    # Path to the JSON file containing the sub account key to use to sign oracle votes\
    sub_account_key_file_path = "config/oracle_sub_account_key.json"
    ' $CARBON_HOME_PATH/config/config.toml
    ```

  * #### ii) Run the following command to verify if the configs were added successfully.

    ```bash
    cat $CARBON_HOME_PATH/config/config.toml | grep sub_account
    ```

    You should get the following output:

    ```bash
    enable_sub_account_signing = true
    sub_account_key_file_path = "config/oracle_sub_account_key.json"
    ```


## 2. Add oracle sub account credentials, in your **VALIDATOR NODE**

  **:warning: If you have not created an oracle sub account, follow the instructions [here]https://github.com/Switcheo/carbon-bootstrap/blob/master/KEYS.md#create-oracle-subaccount-key to create one: :warning:**

  * #### i) Get your oracle sub account key credentials.
    
    You will need to get your oracle sub account address in HEX, public key in BASE64, and private key in BASE64.

    **:warning: Take down all these values as you will need it later. :warning:**

    To get your address in HEX, run:
    ```bash
    carbond keys parse < BECH32_ORACLE_SUB_ACCOUNT_ADDRESS>
    ```

    Example:
    ```bash
    carbond keys parse swth1m9h80cfhqn08x7hull0n7pwhlu9k0vp7hjm3rp
    ```

    To get your public key in HEX, run:
    ```bash
    carbond keys show <ORACLE_SUB_ACCOUNT_NAME> --keyring-backend file
    ```

    Example:
    ```bash
    carbond keys show oracle --keyring-backend file
    ```

    To get your private key, run:
    ```bash
    carbond keys export <ORACLE_SUB_ACCOUNT_NAME> --keyring-backend file --unsafe --unarmored-hex
    ```

    **:warning: Exporting of the private key will be a HEX value, you will have to convert it into BASE64 after :warning:**

    Example:
    ```bash
    carbond keys export oracle --keyring-backend file --unsafe --unarmored-hex
    ```

  * #### ii) Add your oracle sub account key credentials.
    
    Name the file `oracle_sub_account_key.json` and add it in your `.carbon/config` directory.

    **:warning: Follow the example key format below :warning:**

    ```bash
    {
      "address": "D96E77E13704DE737AFCFFDF3F05D7FF0B67B03E", # in HEX format
      "pub_key": {
        "type": "tendermint/PubKeySecp256k1",
        "value'": "2W534TcE3nN6/P/fPwXX/wtnsD4=" # in BASE64 format
      },
      "priv_key": {
        "type": "tendermint/PrivKeySecp256k1",
        "value'": "2W534TcE3nN6/P/fPwXX/wtnsD4=" # in BASE64 format
      }
    }
    ```

  * #### iii) Run the following command to verify if the key file was created successfully.
    
    ```bash
    cat ~/.carbon/config/oracle_sub_account_key.json
    ```

    You should get a similar output:

    ```bash
    {
      "address": "D96E77E13704DE737AFCFFDF3F05D7FF0B67B03E",
      "pub_key": {
        "type": "tendermint/PubKeySecp256k1",
        "value'": "2W534TcE3nN6/P/fPwXX/wtnsD4="
      },
      "priv_key": {
        "type": "tendermint/PrivKeySecp256k1",
        "value'": "2W534TcE3nN6/P/fPwXX/wtnsD4="
      }
    }
    ```