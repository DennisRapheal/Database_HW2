# Database Hw2

## Homework Instruction

[https://docs.google.com/document/d/1l8nmVrtGr6Gf7mwXJiH5FvXUnS76zXsLQ2nR1UMg3BM/edit](https://docs.google.com/document/d/1l8nmVrtGr6Gf7mwXJiH5FvXUnS76zXsLQ2nR1UMg3BM/edit)

## Code Book

[](https://github.com/OxCGRT/covid-policy-tracker/tree/master/documentation)

## Create a Relational Model

1. Ignore the empty columns. 
    - remove `Region Name`, `Region Code`, and `M1_wildcard`
2. Build the E-R diagram to show the relations
    - **E-R diagram**
        - Diagram:
            
            ![資料庫作業二.drawio (3).png][(Database%20Hw2%208a94cb8d4c2c42899ea6976ea6494984/%25E8%25B3%2587%25E6%2596%2599%25E5%25BA%25AB%25E4%25BD%259C%25E6%25A5%25AD%25E4%25BA%258C.drawio_(3).png)](https://github.com/DennisRapheal/Database_HW2/blob/main/readme_image/%25E8%25B3%2587%25E6%2596%2599%25E5%25BA%25AB%25E4%25BD%259C%25E6%25A5%25AD%25E4%25BA%258C.drawio_(3).png)
            
        - I create a new entity called ***Policy*** to show record the `date` and `jurisdiction`
            - relations in format:  `policy_<type>` indicate the relation between ***Policy*** and those weak entities.
            - Those weak entities were designed according to the different analogies of policies, and one policy has a unique combination of them.
        - ***Area*** is created based on the following issue:
            - a country can be across two different continents:
                - An example:
                    
                    
                    | Europe | EU | Azerbaijan | AZ | AZE | 31 |
                    | --- | --- | --- | --- | --- | --- |
                    | Asia | AS | Azerbaijan | AZ | AZE | 31 |
            - Hence, ***Area*** depends on a unique pair of country and continent
        - Some attributes in ***Policy Indices*** were calculated by the parameters in other weak entities based on the code book, so I assign the relation corresponding to the givdn formula.
3. Turn E-R model into Relational model
    - **Database-diagram:**
        - Diagram:
        
        ![table_diagram.png](Database%20Hw2%208a94cb8d4c2c42899ea6976ea6494984/table_diagram.png)
        
        - the table is created based on the previous E-R diagram.
4. Indicate FD to each table.
    - **Functional Dependencies( Tables ):**
        - After set up all functional dependencies( FDs ), I apply “Simplified test” to all tables and show the from of them in Verification.
        - ***Continent:***
            - attributes:
                
                ```
                Continent_Code
                Continent_Name
                ```
                
            - FDs:
                1. Continent_Code → Continent_Name
                2. Continent_Name → Continent_Code
            - Verification:
                - Candidate_Key:
                    - Continent_Code
                    - Continent_Name
                - This is **3NF** but not **BCNF**!
        - ***Area:***
            - attributes:
                
                ```sql
                AreaID
                Continent_Code
                Country_Name
                ```
                
            - FDs:
                
                AreaID → ( Continent_Code, Country_Name )
                
            - Verification
                - in BCNF
        - ***Country:***
            - attributes:
                
                ```
                Country_Number (last three are null)
                Country_Name
                Two_Letter_Country_Code
                Three_Letter_Country_Code (last three are null)
                ```
                
            - FDs
                1. Country_Name → Country_Number, Two_Letter_Country_Code, Three_Letter_Country_Code
                2. Two_Letter_Country_Code → Country_Name, Country_Number, Three_Letter_Country_Code
            - Verification:
                - Candidate Key:
                    - ( Two_Letter_Country_Code, Continent_Code )
                    - ( Country_Name, Continent_Code )
                - This is **BCNF**!
        - ***Policy:***
            - attributes:
                
                ```
                PolicyID
                Date
                Two_Letter_Country_Code
                Jurisdiction
                ```
                
            - FDs
                1. PolicyID → ( Two_Letter_Country_Code, Jurisdiction, Date )
                2. ( Date, Two_Letter_Country_Code ) → PolicyID
            - Verification:
                - Candidate key:
                    - PolicyID
                - **in BCNF**
        - ***Policies_Indices:***
            - attributes:
                
                ```
                PolicyID
                StringencyIndex_Average
                StringencyIndex_Average_ForDisplay
                GovernmentResponseIndex_Average
                GovernmentResponseIndex_Average_ForDisplay
                ContainmentHealthIndex_Average
                ContainmentHealthIndex_Average_ForDisplay
                EconomicSupportIndex
                EconomicSupportIndex_ForDisplay
                ```
                
            - FDs:
                1. PolicyID → ( all other attributes )
            - Verification:
                - **in BCNF**
        - ***Vaccination_Policies:***
            - attributes:
                
                ```
                PolicyID
                V1_Vaccine_Prioritisation_summary
                V2A_Vaccine_Availability_summary
                V2B_Vaccine_age
                V2C_Vaccine_age
                V2D_MedicallySLASH_clinically_vulnerable_Non_elderly
                V2E_Education
                V2F_Frontline_workers_non_healthcare
                V2G_Frontline_workers_healthcare
                V3_Vaccine_Financial_Support_summary
                V4_Mandatory_Vaccination_summary
                ```
                
            - FDs:
                1. PolicyID → ( all other attributes )
            - Verification:
                - **in BCNF**
        - ***Economic_Policies:***
            - attributes:
                
                ```
                PolicyID
                E1_Income_support
                E1_Flag
                E2_Debt/contract_relief
                E3_Fiscal_measures
                E4_International_support
                ```
                
            - FDs:
                1. PolicyID → ( all other attributes )
            - Verification:
                - **in BCNF**
        - ***Additional_Indices:***
            - attributes
                
                ```
                PolicyID
                ConfirmedCases
                ConfirmedDeaths
                MajorityVaccinated
                ```
                
            - FDs:
                1. PolicyID → ( all other attributes )
            - Verification:
                - It is BCNF
        - ***Vaccinated:***
            - attributes
                
                ```
                MajorityVaccinated
                PopulationVaccinated
                ```
                
            - FDs:
                1. MajorityVaccinated → PopulationVaccinated
            - Verification
                - It is in BCNF
        - ***Health_System_Policies:***
            - attributes:
                
                ```
                PolicyID
                H1_Public_information_campaigns
                H1_Flag
                H2_Testing_policy
                H3_Contact_tracing
                H4_Emergency_investment_in_healthcare
                H5_Investment_in_vaccines
                H6M_Facial_Coverings
                H6M_Flag
                H7_Vaccination_policy
                H7_Flag
                H8M_Protection_of_elderly_people
                H8M_Flag
                ```
                
            - FDs:
                1. PolicyID → ( all other attributes )
            - Verification:
                - **in BCNF**
        - ***Containment_and_closure_Policies:***
            - attributes:
                
                ```
                PolicyID
                C1M_School_closing
                C1M_Flag
                C2M_Workplace_closing
                C2M_Flag
                C3M_Cancel_public_events	
                C3M_Flag	
                C4M_Restrictions_on_gatherings	
                C4M_Flag	
                C5M_Close_public_transport	
                C5M_Flag	
                C6M_Stay_at_home_requirements	
                C6M_Flag	
                C7M_Restrictions_on_internal_movement	
                C7M_Flag
                C8EV_International_travel_controls
                ```
                
            - FDs:
                1. PolicyID → ( all other attributes )
            - Verification:
                - **in BCNF**

## Creating Database Schema and Tables

1. Setting up AWS
    - **building a database:**
        - 建立資料庫
            - auto generated password:
                - database1 password：
                    
                    ```
                    ufpi6vd5eBSEy99uumcX
                    ```
                    
2. Import two given csv files as ***original_table*** and ***original_table2***. 
    - **Import the whole table:**
        
        ```sql
        create table original_table(
        	id SERIAL PRIMARY NOT NULL KEY,
        	CountryName varchar(100),
        	CountryCode char(3),
        	RegionName char(3),
        	RegionCode char(3),
        	Jurisdiction VARCHAR(20),
        	Date char(8),
        	C1M_School_closing FLOAT(2),
        	C1M_Flag INT,
        	C2M_Workplace_closing FLOAT(2),
        	C2M_Flag INT,
        	C3M_Cancel_public_events FLOAT(2),	
        	C3M_Flag INT,	
        	C4M_Restrictions_on_gatherings FLOAT(2),	
        	C4M_Flag INT,	
        	C5M_Close_public_transport FLOAT(2),	
        	C5M_Flag INT,	
        	C6M_Stay_at_home_requirements FLOAT(2),	
        	C6M_Flag INT,	
        	C7M_Restrictions_on_internal_movement FLOAT(2),	
        	C7M_Flag INT,
        	C8EV_International_travel_controls FLOAT(2),	
        	E1_Income_support FLOAT(2),
        	E1_Flag FLOAT(2),
          E2_DebtSLASHcontract_relief FLOAT(2),
        	E3_Fiscal_measures FLOAT(2),
        	E4_International_support FLOAT(2),
        	H1_Public_information_campaigns FLOAT(2),
        	H1_Flag INT,
        	H2_Testing_policy FLOAT(2),
        	H3_Contact_tracing FLOAT(2),
        	H4_Emergency_investment_in_healthcare FLOAT(2),
        	H5_Investment_in_vaccines FLOAT(2),
        	H6M_Facial_Coverings FLOAT(2),
        	H6M_Flag INT,
        	H7_Vaccination_policy FLOAT(2),
        	H7_Flag INT,
        	H8M_Protection_of_elderly_people FLOAT(2),
        	H8M_Flag INT,	
        	M1_Wildcard char(3),
        	V1_Vaccine_Prioritisation_summary INT,
        	V2A_Vaccine_Availability_summary INT,
        	V2B_Vaccine_age VARCHAR(15),
        	V2C_Vaccine_age VARCHAR(15),
        	V2D_MedicallySLASH_clinically_vulnerable_Non_elderly INT,
        	V2E_Education INT,
        	V2F_Frontline_workers_non_healthcare INT,
        	V2G_Frontline_workers_healthcare INT,
        	V3_Vaccine_Financial_Support_summary INT,
        	V4_Mandatory_Vaccination_summary INT,
        	ConfirmedCases INT,
        	ConfirmedDeaths INT,
        	MajorityVaccinated VARCHAR(2),
        	PopulationVaccinated FLOAT(2),	
        	StringencyIndex_Average FLOAT(2),
        	StringencyIndex_Average_ForDisplay FLOAT(2),
        	GovernmentResponseIndex_Average FLOAT(2),
        	GovernmentResponseIndex_Average_ForDisplay FLOAT(2),
        	ContainmentHealthIndex_Average FLOAT(2),
        	ContainmentHealthIndex_Average_ForDisplay FLOAT(2),
        	EconomicSupportIndex FLOAT(2),
        	EconomicSupportIndex_ForDisplay FLOAT(2)
        );
        ```
        
        ```sql
        create table original(
        	CountryName varchar(100),
        	CountryCode char(3),
        	RegionName char(3),
        	RegionCode char(3),
        	Jurisdiction VARCHAR(20),
        	Date char(8),
        	C1M_School_closing FLOAT(2),
        	C1M_Flag INT,
        	C2M_Workplace_closing FLOAT(2),
        	C2M_Flag INT,
        	C3M_Cancel_public_events FLOAT(2),	
        	C3M_Flag INT,	
        	C4M_Restrictions_on_gatherings FLOAT(2),	
        	C4M_Flag INT,	
        	C5M_Close_public_transport FLOAT(2),	
        	C5M_Flag INT,	
        	C6M_Stay_at_home_requirements FLOAT(2),	
        	C6M_Flag INT,	
        	C7M_Restrictions_on_internal_movement FLOAT(2),	
        	C7M_Flag INT,
        	C8EV_International_travel_controls FLOAT(2),	
        	E1_Income_support FLOAT(2),
        	E1_Flag FLOAT(2),
          E2_DebtSLASHcontract_relief FLOAT(2),
        	E3_Fiscal_measures FLOAT(2),
        	E4_International_support FLOAT(2),
        	H1_Public_information_campaigns FLOAT(2),
        	H1_Flag INT,
        	H2_Testing_policy FLOAT(2),
        	H3_Contact_tracing FLOAT(2),
        	H4_Emergency_investment_in_healthcare FLOAT(2),
        	H5_Investment_in_vaccines FLOAT(2),
        	H6M_Facial_Coverings FLOAT(2),
        	H6M_Flag INT,
        	H7_Vaccination_policy FLOAT(2),
        	H7_Flag INT,
        	H8M_Protection_of_elderly_people FLOAT(2),
        	H8M_Flag INT,	
        	M1_Wildcard char(3),
        	V1_Vaccine_Prioritisation_summary INT,
        	V2A_Vaccine_Availability_summary INT,
        	V2B_Vaccine_age VARCHAR(15),
        	V2C_Vaccine_age VARCHAR(15),
        	V2D_MedicallySLASH_clinically_vulnerable_Non_elderly INT,
        	V2E_Education INT,
        	V2F_Frontline_workers_non_healthcare INT,
        	V2G_Frontline_workers_healthcare INT,
        	V3_Vaccine_Financial_Support_summary INT,
        	V4_Mandatory_Vaccination_summary INT,
        	ConfirmedCases INT,
        	ConfirmedDeaths INT,
        	MajorityVaccinated VARCHAR(2),
        	PopulationVaccinated FLOAT(2),	
        	StringencyIndex_Average FLOAT(2),
        	StringencyIndex_Average_ForDisplay FLOAT(2),
        	GovernmentResponseIndex_Average FLOAT(2),
        	GovernmentResponseIndex_Average_ForDisplay FLOAT(2),
        	ContainmentHealthIndex_Average FLOAT(2),
        	ContainmentHealthIndex_Average_ForDisplay FLOAT(2),
        	EconomicSupportIndex FLOAT(2),
        	EconomicSupportIndex_ForDisplay FLOAT(2)
        );
        ```
        
        ```sql
        \copy original from '/Users/lintingwei/Desktop/oxcgrt_dataset/OxCGRT_nat_latest.csv' delimiter ',' CSV HEADER;
        ```
        
        ```sql
        insert into original_table(
        	CountryName ,
        	CountryCode ,
        	RegionName ,
        	RegionCode ,
        	Jurisdiction ,
        	Date,
        	C1M_School_closing ,
        	C1M_Flag,
        	C2M_Workplace_closing,
        	C2M_Flag ,
        	C3M_Cancel_public_events,	
        	C3M_Flag ,	
        	C4M_Restrictions_on_gatherings,	
        	C4M_Flag ,	
        	C5M_Close_public_transport,	
        	C5M_Flag ,	
        	C6M_Stay_at_home_requirements,	
        	C6M_Flag,	
        	C7M_Restrictions_on_internal_movement,	
        	C7M_Flag,
        	C8EV_International_travel_controls,	
        	E1_Income_support,
        	E1_Flag,
          E2_DebtSLASHcontract_relief,
        	E3_Fiscal_measures ,
        	E4_International_support ,
        	H1_Public_information_campaigns,
        	H1_Flag,
        	H2_Testing_policy,
        	H3_Contact_tracing,
        	H4_Emergency_investment_in_healthcare,
        	H5_Investment_in_vaccines,
        	H6M_Facial_Coverings,
        	H6M_Flag,
        	H7_Vaccination_policy,
        	H7_Flag,
        	H8M_Protection_of_elderly_people,
        	H8M_Flag ,	
        	M1_Wildcard ,
        	V1_Vaccine_Prioritisation_summary,
        	V2A_Vaccine_Availability_summary,
        	V2B_Vaccine_age,
        	V2C_Vaccine_age ,
        	V2D_MedicallySLASH_clinically_vulnerable_Non_elderly ,
        	V2E_Education ,
        	V2F_Frontline_workers_non_healthcare ,
        	V2G_Frontline_workers_healthcare ,
        	V3_Vaccine_Financial_Support_summary ,
        	V4_Mandatory_Vaccination_summary ,
        	ConfirmedCases ,
        	ConfirmedDeaths ,
        	MajorityVaccinated,
        	PopulationVaccinated,	
        	StringencyIndex_Average ,
        	StringencyIndex_Average_ForDisplay,
        	GovernmentResponseIndex_Average ,
        	GovernmentResponseIndex_Average_ForDisplay,
        	ContainmentHealthIndex_Average,
        	ContainmentHealthIndex_Average_ForDisplay ,
        	EconomicSupportIndex,
        	EconomicSupportIndex_ForDisplay)
        select *
        from original;
        ```
        
        ```sql
        create table original_table2(
        	Continent_Name varchar(100),
        	Continent_Code char(2),
        	Country_Name VARCHAR(100),
        	Two_Letter_Country_Code CHAR(2),
        	Three_Letter_Country_Code CHAR(3),	
        	Country_Number INT
        );
        ```
        
        ```sql
        \copy original_table2 from '/Users/lintingwei/Desktop/oxcgrt_dataset/country-and-continent-codes-list-csv.csv' delimiter ',' CSV HEADER;
        ```
        
3. Create tables respectively based on the [database-diagram](https://www.notion.so/Database-Hw2-8a94cb8d4c2c42899ea6976ea6494984?pvs=21)
    - **Creation of All Tables ( Code Implementation ):**
        - ***Continent***
            - create table:
                
                ```sql
                create table Continent(
                	Continent_Code char(2),
                	Continent_Name varchar(100),
                	primary key(Continent_Code)
                );
                ```
                
                ```sql
                insert into continent(Continent_Code, Continent_Name)
                select distinct Continent_Code, Continent_Name
                from original_table2;
                ```
                
        - ***Area***
            - create table
                
                ```sql
                CREATE TABLE Area(
                	AreaID SERIAL PRIMARY KEY,
                  Two_Letter_Country_Code CHAR(2),
                	Continent_Code CHAR(2),
                	foreign key (Two_Letter_Country_Code) references Country(Two_Letter_Country_Code),
                	foreign key (Continent_Code) references Continent(Continent_Code)
                );
                ```
                
                ```sql
                insert into Area(Two_Letter_Country_Code, Continent_Code)
                select Two_Letter_Country_Code, Continent_Code
                from original_table2;
                ```
                
        - ***Country***
            - create table
                
                ```sql
                CREATE TABLE Country(
                	Country_Number INT,
                	Country_Name VARCHAR(100),
                	Two_Letter_Country_Code CHAR(2),
                	Three_Letter_Country_Code CHAR(3),
                	primary key (Two_Letter_Country_Code)
                );
                ```
                
                ```sql
                UPDATE original_table2
                SET Country_Name = original_table.CountryName
                FROM original_table
                WHERE original_table2.Three_Letter_Country_Code = original_table.CountryCode;
                ```
                
                ```sql
                insert into Country(Country_Name, Country_Number, Two_Letter_Country_Code,Three_Letter_Country_Code )
                select distinct Country_Name, Country_Number, Two_Letter_Country_Code,Three_Letter_Country_Code
                from original_table2;
                ```
                
        - ***Policy***
            - create table
                
                ```sql
                CREATE TABLE Policy(
                	PolicyID int PRIMARY KEY,
                	Date char(8),
                	Two_Letter_Country_Code CHAR(2),
                	Jurisdiction VARCHAR(20),
                	foreign key (Two_Letter_Country_Code) references Country(Two_Letter_Country_Code),
                	foreign key (PolicyID) references Additional_Indices(PolicyID),
                	foreign key (PolicyID) references Economic_Policies(PolicyID),
                	foreign key (PolicyID) references Containment_and_closure_policies(PolicyID),
                	foreign key (PolicyID) references Health_System_Policies(PolicyID),
                	foreign key (PolicyID) references Vaccination_Policies(PolicyID),
                	foreign key (PolicyID) references Policy_Indices(PolicyID)
                );
                ```
                
                ```sql
                INSERT INTO Policy(policyID, Date, Two_Letter_Country_Code, Jurisdiction)
                SELECT original_table.id, original_table.Date, country.Two_Letter_Country_Code, original_table.Jurisdiction
                FROM original_table LEFT JOIN country
                ON original_table.CountryCode = country.Three_Letter_Country_Code;
                ```
                
        - ***Additional_Indices***
            - create table
                
                ```sql
                CREATE TABLE Additional_Indices(
                	PolicyID INT PRIMARY KEY,
                	ConfirmedCases FLOAT,
                	ConfirmedDeaths INT,
                	PopulationVaccinated FLOAT(2)
                );
                ALTER TABLE Additional_Indices ADD CONSTRAINT PopulationVaccinated FOREIGN KEY (PopulationVaccinated) REFERENCES ***Vaccinated***(PopulationVaccinated);
                ```
                
                ```sql
                insert into Additional_Indices(PolicyID, ConfirmedCases, ConfirmedDeaths, PopulationVaccinated)
                select id, ConfirmedCases, ConfirmedDeaths, PopulationVaccinated
                from original_table;
                ```
                
        - ***Vaccinated:***
            - create table
                
                ```sql
                CREATE TABLE Vaccinated(
                	PopulationVaccinated FLOAT(2) PRIMARY KEY,
                	MajorityVaccinated VARCHAR(2) 
                );
                ```
                
                ```sql
                INSERT INTO Vaccinated(PopulationVaccinated, MajorityVaccinated)
                select distinct PopulationVaccinated, MajorityVaccinated
                from original_table
                WHERE PopulationVaccinated IS NOT NULL;;
                ```
                
        - ***Economic_Policies***
            - create table
                
                ```sql
                CREATE TABLE Economic_Policies(
                	PolicyID INT PRIMARY KEY,
                	E1_Income_support FLOAT(2),
                	E1_Flag FLOAT(2),
                  E2_DebtSLASHcontract_relief FLOAT(2),
                	E3_Fiscal_measures FLOAT(2),
                	E4_International_support FLOAT(2)
                );
                ```
                
                ```sql
                INSERT INTO Economic_Policies(PolicyID, E1_Income_support, E1_Flag, E2_DebtSLASHcontract_relief,E3_Fiscal_measures,E4_International_support)
                SELECT id, E1_Income_support, E1_Flag, E2_DebtSLASHcontract_relief,E3_Fiscal_measures,E4_International_support
                FROM original_table;
                ```
                
        - ***Containment_and_closure_policies***
            - create table
                
                ```sql
                CREATE TABLE Containment_and_closure_policies(
                	PolicyID INT PRIMARY KEY,
                	C1M_School_closing FLOAT(2),
                	C1M_Flag INT,
                	C2M_Workplace_closing FLOAT(2),
                	C2M_Flag INT,
                	C3M_Cancel_public_events FLOAT(2),	
                	C3M_Flag INT,	
                	C4M_Restrictions_on_gatherings FLOAT(2),	
                	C4M_Flag INT,	
                	C5M_Close_public_transport FLOAT(2),	
                	C5M_Flag INT,	
                	C6M_Stay_at_home_requirements FLOAT(2),	
                	C6M_Flag INT,	
                	C7M_Restrictions_on_internal_movement FLOAT(2),	
                	C7M_Flag INT,
                	C8EV_International_travel_controls FLOAT(2)
                );
                ```
                
                ```sql
                INSERT INTO Containment_and_closure_policies(
                	PolicyID,
                	C1M_School_closing, 
                	C1M_Flag, 
                	C2M_Workplace_closing,
                	C2M_Flag, 
                	C3M_Cancel_public_events, 
                	C3M_Flag,	
                	C4M_Restrictions_on_gatherings,	
                	C4M_Flag,	
                	C5M_Close_public_transport,	
                	C5M_Flag,
                	C6M_Stay_at_home_requirements,	
                	C6M_Flag,	
                	C7M_Restrictions_on_internal_movement,	
                	C7M_Flag, 
                	C8EV_International_travel_controls)
                SELECT 
                	id, 
                	C1M_School_closing, 
                	C1M_Flag, 
                	C2M_Workplace_closing,
                	C2M_Flag, 
                	C3M_Cancel_public_events, 
                	C3M_Flag,	
                	C4M_Restrictions_on_gatherings,	
                	C4M_Flag,	
                	C5M_Close_public_transport,	
                	C5M_Flag,
                	C6M_Stay_at_home_requirements,	
                	C6M_Flag,	
                	C7M_Restrictions_on_internal_movement,	
                	C7M_Flag, 
                	C8EV_International_travel_controls
                FROM original_table;
                ```
                
        - ***Health_System_Policies***
            - create table
                
                ```sql
                CREATE TABLE Health_System_Policies(
                	PolicyID INT PRIMARY KEY,
                	H1_Public_information_campaigns FLOAT(2),
                	H1_Flag INT,
                	H2_Testing_policy FLOAT(2),
                	H3_Contact_tracing FLOAT(2),
                	H4_Emergency_investment_in_healthcare FLOAT(2),
                	H5_Investment_in_vaccines FLOAT(2),
                	H6M_Facial_Coverings FLOAT(2),
                	H6M_Flag INT,
                	H7_Vaccination_policy FLOAT(2),
                	H7_Flag INT,
                	H8M_Protection_of_elderly_people FLOAT(2),
                	H8M_Flag INT
                );
                ```
                
                ```sql
                INSERT INTO Health_System_Policies(
                	PolicyID,
                	H1_Public_information_campaigns,
                	H1_Flag,
                	H2_Testing_policy,
                	H3_Contact_tracing,
                	H4_Emergency_investment_in_healthcare,
                	H5_Investment_in_vaccines,
                	H6M_Facial_Coverings,
                	H6M_Flag,
                	H7_Vaccination_policy,
                	H7_Flag,
                	H8M_Protection_of_elderly_people,
                	H8M_Flag
                )
                SELECT 
                	id, 
                	H1_Public_information_campaigns,
                	H1_Flag,
                	H2_Testing_policy,
                	H3_Contact_tracing,
                	H4_Emergency_investment_in_healthcare,
                	H5_Investment_in_vaccines,
                	H6M_Facial_Coverings,
                	H6M_Flag,
                	H7_Vaccination_policy,
                	H7_Flag,
                	H8M_Protection_of_elderly_people,
                	H8M_Flag
                FROM original_table;
                ```
                
        - ***Vaccination_Policies***
            - create table
                
                ```sql
                CREATE TABLE Vaccination_Policies(
                	PolicyID INT PRIMARY KEY,
                	V1_Vaccine_Prioritisation_summary INT,
                	V2A_Vaccine_Availability_summary INT,
                	V2B_Vaccine_age VARCHAR(15),
                	V2C_Vaccine_age VARCHAR(15),
                	V2D_MedicallySLASH_clinically_vulnerable_Non_elderly INT,
                	V2E_Education INT,
                	V2F_Frontline_workers_non_healthcare INT,
                	V2G_Frontline_workers_healthcare INT,
                	V3_Vaccine_Financial_Support_summary INT,
                	V4_Mandatory_Vaccination_summary INT
                );
                ```
                
                ```sql
                INSERT INTO Vaccination_Policies(
                	PolicyID,
                	V1_Vaccine_Prioritisation_summary,
                	V2A_Vaccine_Availability_summary,
                	V2B_Vaccine_age,
                	V2C_Vaccine_age,
                	V2D_MedicallySLASH_clinically_vulnerable_Non_elderly,
                	V2E_Education,
                	V2F_Frontline_workers_non_healthcare,
                	V2G_Frontline_workers_healthcare,
                	V3_Vaccine_Financial_Support_summary,
                	V4_Mandatory_Vaccination_summary)
                SELECT 
                	id, 
                	V1_Vaccine_Prioritisation_summary,
                	V2A_Vaccine_Availability_summary,
                	V2B_Vaccine_age,
                	V2C_Vaccine_age,
                	V2D_MedicallySLASH_clinically_vulnerable_Non_elderly,
                	V2E_Education,
                	V2F_Frontline_workers_non_healthcare,
                	V2G_Frontline_workers_healthcare,
                	V3_Vaccine_Financial_Support_summary,
                	V4_Mandatory_Vaccination_summary
                FROM original_table;
                ```
                
        - ***Policy_Indices***
            - create table
                
                ```sql
                CREATE TABLE Policy_Indices(
                	PolicyID INT PRIMARY KEY,
                	StringencyIndex_Average FLOAT(2),
                	StringencyIndex_Average_ForDisplay FLOAT(2),
                	GovernmentResponseIndex_Average FLOAT(2),
                	GovernmentResponseIndex_Average_ForDisplay FLOAT(2),
                	ContainmentHealthIndex_Average FLOAT(2),
                	ContainmentHealthIndex_Average_ForDisplay FLOAT(2),
                	EconomicSupportIndex FLOAT(2),
                	EconomicSupportIndex_ForDisplay FLOAT(2)
                );
                ```
                
                ```sql
                INSERT INTO Policy_Indices(
                	PolicyID,
                	StringencyIndex_Average,
                	StringencyIndex_Average_ForDisplay,
                	GovernmentResponseIndex_Average,
                	GovernmentResponseIndex_Average_ForDisplay,
                	ContainmentHealthIndex_Average,
                	ContainmentHealthIndex_Average_ForDisplay,
                	EconomicSupportIndex,
                	EconomicSupportIndex_ForDisplay)
                SELECT 
                	id, 
                	StringencyIndex_Average,
                	StringencyIndex_Average_ForDisplay,
                	GovernmentResponseIndex_Average,
                	GovernmentResponseIndex_Average_ForDisplay,
                	ContainmentHealthIndex_Average,
                	ContainmentHealthIndex_Average_ForDisplay,
                	EconomicSupportIndex,
                	EconomicSupportIndex_ForDisplay
                FROM original_table;
                ```
                
        

## Query Implementation

- 4-a
    - Question:
        - List the `country names`, `continents`, and `date` of the countries with the highest and lowest `Stringency Index` on 2022/12/01, 2022/04/01, 2021/04/01, and 2020/04/01 in each continent. If there are multiple Stringency Indexes in one country per day, please aggregate them with a reasonable approach and explain why you chose the method ( if there is only one `Stringency Index` in one country per day, then you can use `StringencyIndex_Average_ForDisplay directly`). Please use `StringencyIndex_Average_ForDisplay` column in this Query.
    - Query:
        
        ```sql
        WITH basedata(country2W,policyID, StringencyIndex, date) AS (
        	SELECT policy.Two_Letter_Country_Code, policy.policyID
        				, Policy_indices.StringencyIndex_Average_ForDisplay, policy.date
        	FROM policy join Policy_indices
        	ON policy.policyID = Policy_indices.policyID),
        countryInfo(countryname, continent, country2W) AS(
        	SELECT country.country_name, continent.continent_name, country.Two_Letter_Country_Code
        	FROM country JOIN area ON country.Two_Letter_Country_Code = area.Two_Letter_Country_Code
        							JOIN continent ON area.continent_code = continent.continent_code
        ),
        day1(country2W, policyID, StringencyIndex, date) AS(
        	SELECT country2W, policyID, StringencyIndex, date
        	FROM basedata
        	WHERE date = '20221201'),
        day2(country2W, policyID, StringencyIndex, date) AS(
        	SELECT country2W, policyID, StringencyIndex, date
        	FROM basedata
        	WHERE date = '20220401'),
        day3(country2W, policyID, StringencyIndex, date) AS(
        	SELECT country2W, policyID, StringencyIndex, date
        	FROM basedata
        	WHERE date = '20210401'),
        day4(country2W, policyID, StringencyIndex, date) AS(
        	SELECT country2W, policyID, StringencyIndex, date
        	FROM basedata
        	WHERE date = '20200401'),
        day1_Max(continent, StringencyIndex) AS(
        	SELECT continent, max(StringencyIndex)
        	FROM day1 JOIN countryInfo ON day1.country2w = countryInfo.country2w
        	GROUP BY continent),
        day1_min(continent, StringencyIndex) AS(
        	SELECT continent, min(StringencyIndex)
        	FROM day1 JOIN countryInfo ON day1.country2w = countryInfo.country2w
        	GROUP BY continent),
        day2_Max(continent, StringencyIndex) AS(
        	SELECT continent, max(StringencyIndex)
        	FROM day2 JOIN countryInfo ON day2.country2w = countryInfo.country2w
        	GROUP BY continent),
        day2_min(continent, StringencyIndex) AS(
        	SELECT continent, min(StringencyIndex)
        	FROM day2 JOIN countryInfo ON day2.country2w = countryInfo.country2w
        	GROUP BY continent),
        day3_Max(continent, StringencyIndex) AS(
        	SELECT continent, max(StringencyIndex)
        	FROM day3 JOIN countryInfo ON day3.country2w = countryInfo.country2w
        	GROUP BY continent),
        day3_min(continent, StringencyIndex) AS(
        	SELECT continent, min(StringencyIndex)
        	FROM day3 JOIN countryInfo ON day3.country2w = countryInfo.country2w
        	GROUP BY continent),
        day4_Max(continent, StringencyIndex) AS(
        	SELECT continent, max(StringencyIndex)
        	FROM day4 JOIN countryInfo ON day4.country2w = countryInfo.country2w
        	GROUP BY continent),
        day4_min(continent, StringencyIndex) AS(
        	SELECT continent, min(StringencyIndex)
        	FROM day4 JOIN countryInfo ON day4.country2w = countryInfo.country2w
        	GROUP BY continent)
        SELECT day1_min.continent, countryname, day1_min.StringencyIndex, date
        from day1_min 
        	JOIN day1 ON day1.StringencyIndex = day1_min.StringencyIndex
        	JOIN countryInfo ON day1.country2W = countryInfo.country2w 
        				and countryInfo.continent = day1_min.continent
        UNION
        SELECT day1_max.continent, countryname, day1_max.StringencyIndex as 'Over Stringency Index', date
        from day1_max 
        	JOIN day1 ON day1.StringencyIndex = day1_max.StringencyIndex
        	JOIN countryInfo ON day1.country2W = countryInfo.country2w 
        				and countryInfo.continent = day1_max.continent
        UNION
        SELECT day2_min.continent, countryname, day2_min.StringencyIndex as 'Over Stringency Index', date
        from day2_min 
        	JOIN day2 ON day2.StringencyIndex = day2_min.StringencyIndex
        	JOIN countryInfo ON day2.country2W = countryInfo.country2w 
        				and countryInfo.continent = day2_min.continent
        UNION
        SELECT day2_max.continent, countryname, day2_max.StringencyIndex as 'Over Stringency Index', date
        from day2_max 
        	JOIN day2 ON day2.StringencyIndex = day2_max.StringencyIndex
        	JOIN countryInfo ON day2.country2W = countryInfo.country2w 
        				and countryInfo.continent = day2_max.continent
        UNION
        SELECT day3_min.continent, countryname, day3_min.StringencyIndex as 'Over Stringency Index', date
        from day3_min 
        	JOIN day3 ON day3.StringencyIndex = day3_min.StringencyIndex
        	JOIN countryInfo ON day3.country2W = countryInfo.country2w 
        				and countryInfo.continent = day3_min.continent
        UNION
        SELECT day3_max.continent, countryname, day3_max.StringencyIndex as 'Over Stringency Index', date
        from day3_max 
        	JOIN day3 ON day3.StringencyIndex = day3_max.StringencyIndex
        	JOIN countryInfo ON day3.country2W = countryInfo.country2w 
        				and countryInfo.continent = day3_max.continent
        UNION
        SELECT day4_min.continent, countryname, day4_min.StringencyIndex as 'Over Stringency Index', date
        from day4_min 
        	JOIN day4 ON day4.StringencyIndex = day4_min.StringencyIndex
        	JOIN countryInfo ON day4.country2W = countryInfo.country2w 
        				and countryInfo.continent = day4_min.continent
        UNION
        SELECT day4_max.continent, countryname, day4_max.StringencyIndex as 'Over Stringency Index', date
        from day4_max 
        	JOIN day4 ON day4.StringencyIndex = day4_max.StringencyIndex
        	JOIN countryInfo ON day4.country2W = countryInfo.country2w 
        				and countryInfo.continent = day4_max.continent
        ORDER BY continent;
        ```
        
    - Outcome:
        - day1_max and day1_min have no value since there were no record on 2022/12/01
        - day4_max:
            - the country Georgia is across Asia and Europe, so it has two value.
- 4-b
    - Question:
        - We define an indicator to show the “over Stringency index” as ((maybe aggregated) [Stringency Index](https://docs.google.com/document/d/1l8nmVrtGr6Gf7mwXJiH5FvXUnS76zXsLQ2nR1UMg3BM/edit#bookmark=id.l1dsa4qp4i0a))/([7-day moving average](https://docs.google.com/document/d/1l8nmVrtGr6Gf7mwXJiH5FvXUnS76zXsLQ2nR1UMg3BM/edit#bookmark=id.l1dsa4qp4i0a) of daily confirmed new cases). Please list the `country names`, `continents`, “`over Stringency index`”, and `date` with the highest and lowest “over Stringency index” on 2022/12/01, 2022/04/01, 2021/04/01, and 2020/04/01 **in each continent**. Please use `StringencyIndex_Average_ForDisplay` column in this Query. (10pts)
    - Query:
        - Modify the number of confirmed cases :
            - if it is `null` or `0`→ modified to 0.1
                - Since `null` value usually occurred at the beginning of pandemic → no confirmed cases.
            
            ```sql
            UPDATE Additional_Indices
            SET ConfirmedCases = 0.1 
            WHERE ConfirmedCases = 0 OR ConfirmedCases = null;
            ```
            
        - Since a country may lie on two continents, we have to join base and countryInfo first.
            
            ```sql
            with policyAndAdd(country2W, policyID, confirmedCases, date) AS (
            	SELECT policy.Two_Letter_Country_Code, policy.policyID, confirmedCases, policy.date
            	FROM Policy 
            	JOIN Additional_Indices ON Policy.policyID = Additional_Indices.policyID),
            increaseCase(country2W, policyID, increaseCases, date) AS(
            	SELECT country2W, policyID, 
            		confirmedCases - LAG(confirmedCases, 7, confirmedCases) OVER(),date
            	FROM policyAndAdd),
            movingAVG(country2W, policyID, AVGcases, date) AS(
            	SELECT country2W, policyID, case 
            								when increaseCases <= 0 then 0.1
            								when increaseCases = null then 0.1
            								else increaseCases/7
            								end
            	, date
            	FROM increaseCase),
            policyAndpol(policyID, StringencyIndex) AS (
            	SELECT policy.policyID, Policy_indices.StringencyIndex_Average_ForDisplay
            	FROM policy join Policy_indices
            	ON policy.policyID = Policy_indices.policyID),
            countryInfo(countryname, continent, country2W) AS(
            	SELECT country.country_name, continent.continent_name
            					,country.Two_Letter_Country_Code
            	FROM country 
            		JOIN area ON country.Two_Letter_Country_Code = area.Two_Letter_Country_Code
            		JOIN continent ON area.continent_code = continent.continent_code),
            day1(country2W, policyID, OSindex, date) AS(
            	SELECT country2W, movingAVG.policyID, StringencyIndex/AVGcases, date
            	FROM movingAVG JOIN policyAndpol
            	ON policyAndpol.policyID = movingAVG.policyID
            	WHERE date = '20221201'),
            day2(country2W, policyID, OSindex, date) AS(
            	SELECT country2W, movingAVG.policyID, StringencyIndex/AVGcases, date
            	FROM movingAVG JOIN policyAndpol
            	ON policyAndpol.policyID = movingAVG.policyID
            	WHERE date = '20220401'),
            day3(country2W, policyID, OSindex, date) AS(
            	SELECT country2W, movingAVG.policyID, StringencyIndex/AVGcases, date
            	FROM movingAVG JOIN policyAndpol
            	ON policyAndpol.policyID = movingAVG.policyID
            	WHERE date = '20210401'),
            day4(country2W, policyID, OSindex, date) AS(
            	SELECT country2W, movingAVG.policyID, StringencyIndex/AVGcases, date
            	FROM movingAVG JOIN policyAndpol
            	ON policyAndpol.policyID = movingAVG.policyID
            	WHERE date = '20200401'),
            day1_min(continent, OSindex) AS(
            	SELECT continent, min(Osindex)
            	FROM day1 JOIN countryInfo ON day1.country2w = countryInfo.country2w
            	GROUP BY continent),
            day1_Max(continent, OSindex) AS(
            	SELECT continent, max(Osindex)
            	FROM day1 JOIN countryInfo ON day1.country2w = countryInfo.country2w
            	GROUP BY continent),
            day2_min(continent, OSindex) AS(
            	SELECT continent, min(Osindex)
            	FROM day2 JOIN countryInfo ON day2.country2w = countryInfo.country2w
            	GROUP BY continent),
            day2_Max(continent, OSindex) AS(
            	SELECT continent, max(Osindex)
            	FROM day2 JOIN countryInfo ON day2.country2w = countryInfo.country2w
            	GROUP BY continent),
            day3_min(continent, OSindex) AS(
            	SELECT continent, min(Osindex)
            	FROM day3 JOIN countryInfo ON day3.country2w = countryInfo.country2w
            	GROUP BY continent),
            day3_Max(continent, OSindex) AS(
            	SELECT continent, max(Osindex)
            	FROM day3 JOIN countryInfo ON day3.country2w = countryInfo.country2w
            	GROUP BY continent),
            day4_min(continent, OSindex) AS(
            	SELECT continent, min(Osindex)
            	FROM day4 JOIN countryInfo ON day4.country2w = countryInfo.country2w
            	GROUP BY continent),
            day4_Max(continent, OSindex) AS(
            	SELECT continent, max(Osindex)
            	FROM day4 JOIN countryInfo ON day4.country2w = countryInfo.country2w
            	GROUP BY continent)
            SELECT day1_min.continent, countryname, day1_min.Osindex, date
            from day1_min 
            	JOIN day1 ON day1.Osindex = day1_min.osindex
            	JOIN countryInfo ON day1.country2W = countryInfo.country2w 
            					and countryInfo.continent = day1_min.continent
            UNION
            SELECT day1_max.continent, countryname, day1_max.Osindex, date
            from day1_max 
            	JOIN day1 ON day1.Osindex = day1_max.osindex
            	JOIN countryInfo ON day1.country2W = countryInfo.country2w 
            					and countryInfo.continent = day1_max.continent
            UNION
            SELECT day2_min.continent, countryname, day2_min.Osindex, date
            from day2_min 
            	JOIN day2 ON day2.Osindex = day2_min.osindex
            	JOIN countryInfo ON day2.country2W = countryInfo.country2w 
            					and countryInfo.continent = day2_min.continent
            UNION
            SELECT day2_max.continent, countryname, day2_max.Osindex, date
            from day2_max 
            	JOIN day2 ON day2.Osindex = day2_max.osindex
            	JOIN countryInfo ON day2.country2W = countryInfo.country2w 
            					and countryInfo.continent = day2_max.continent
            UNION
            SELECT day3_min.continent, countryname, day3_min.Osindex, date
            from day3_min 
            	JOIN day3 ON day3.Osindex = day3_min.osindex
            	JOIN countryInfo ON day3.country2W = countryInfo.country2w 
            					and countryInfo.continent = day3_min.continent
            UNION
            SELECT day3_max.continent, countryname, day3_max.Osindex, date
            from day3_max 
            	JOIN day3 ON day3.Osindex = day3_max.osindex
            	JOIN countryInfo ON day3.country2W = countryInfo.country2w 
            					and countryInfo.continent = day3_max.continent
            UNION
            SELECT day4_min.continent, countryname, day4_min.Osindex, date
            from day4_min 
            	JOIN day4 ON day4.Osindex = day4_min.osindex
            	JOIN countryInfo ON day4.country2W = countryInfo.country2w 
            					and countryInfo.continent = day4_min.continent
            UNION
            SELECT day4_max.continent, countryname, day4_max.Osindex, date
            from day4_max 
            	JOIN day4 ON day4.Osindex = day4_max.osindex
            	JOIN countryInfo ON day4.country2W = countryInfo.country2w 
            					and countryInfo.continent = day4_max.continent
            order by continent;
            ```
            
    - Outcome:

## Lambda Function

- create instance:
    - name: db-learn-1
