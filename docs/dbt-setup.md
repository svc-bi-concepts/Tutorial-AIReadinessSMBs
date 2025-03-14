# dbt Cloud Setup
!!! note "MFA required"
      With the second sign in, dbt cloud it asking for multi factor authentication.

## 1. Create a dbt Cloud Account
   1. To start using dbt Cloud, sign up for an account on the [dbt Cloud website](https://cloud.getdbt.com/){:target="_blank"}. **Start free trial**.

    !!! note "Note down dbt credentials"
        Add username and password to your text file.


      ![dbt Cloud Homepage](./assets/screenshots/dbtSetup/dbtSetup1.png)

   2. You will receive an email for the verification process.

      ![dbt Cloud Verification Email](./assets/screenshots/dbtSetup/dbtSetup2.png)

      Click on the **Verify your email address** to confirm your account.

      ![dbt Cloud Email Verification](./assets/screenshots/dbtSetup/dbtSetup3.png)

   3. Account setup complete! You can dismiss any pop-ups that might appear.

      ![dbt Cloud Account Setup](./assets/screenshots/dbtSetup/dbtSetup4.png)

---

## 2. Complete the project setup
dbt Cloud will per default set up a new project called `ANALYTICS`, which we will leverage for our lab.

   1. Select dropdown for **"Connection"**. **Add new connection**.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup5.png)

   2. Select your data warehouse `Snowflake`.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup6.png)


   3. Fill out the form .

   - Connection name: `Snowflake_HWZ`
   - Account: `Account Identifier` from your notes in the Snowflake section.
   - Database (Target in Snowflake): `Analytics`
   - Warehouse: `Transformer`

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup7.png)

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup8.png)



---

## 3. Connect dbt Cloud to Snowflake with adding your developer credentials

   Go back to the **Dashboard** on the top left corner to configure your development environment.

   To enable dbt transformations, you need to add the development credentials to your connection for dbt Cloud to your Snowflake instance:

   1. Choose **Snowflake_HWZ** as your Connection.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup9.png)

   2. Enter the required Snowflake credentials:
      - Username from your snowflake account
      - Password from your snowflake account
      - Schema (leave the default: dbt_<yourname>)
      - Target name (leave: `default`)
      - Threads: `16` [^3]
      [^3]: Not essential, but good practice as we do not want to artificially limit Snowflakes capabilities. The smallest warehouse starts with 8 threads.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup10.png)

      **Test Connection** to verify the connection between dbt Cloud and Snowflake works.

   3. Save Development credentials if successful.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup11.png)   



---

## 4. Setup the Project Repository to complete the Project Setup
   For simplicity, we are going to use the dbt Cloud IDE to manage our project repository.

   1. Select **Managed** and add a name for your repository. `analytic-hwz` for example.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup12.png)

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup13.png)

   2. Success - Let's go to **Start developing in the IDE**

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup14.png)

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup15.png)

---

## 5. Initialize your Analytics Project

The initialization creates the following project base structure:

      📦 your_dbt_project
      ├── 📂 analyses
      ├── 📂 macros
      ├── 📂 models
      ├── 📂 seeds
      ├── 📂 snapshots
      ├── 📂 target
      ├── 📂 tests
      ├── 📄 .gitignore
      ├── 📄 README
      └── 📄 dbt_project


   1. Ignore the upcoming pop-ups.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup16.png)

   2. **Initialize dbt project** by clicking on the button in the top left.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup17.png)

   3. Initialization comes with an example model, which we do not care about. **Delete** the example model `./models/example`.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup18.png)

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup19.png)

   4. **Commit and sync** the changes.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup20.png)

      Add a message like `Initial commit` and hit **Commit changes**.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup21.png)

   5. **Success** - your dbt project is now ready for development.

      ![dbt Cloud New Project](./assets/screenshots/dbtSetup/dbtSetup22.png)


---

## 🎉 Next Steps
Now that dbt Cloud is configured, proceed to setting up transformations and version control.

🔗 **Continue to:** [Data loading with Fivetran](fivetran-setup.md)