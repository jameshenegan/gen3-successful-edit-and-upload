# Successful Node Creation and Data Upload

For the initial setup, I mainly followed [this readme by Andre Briggs](https://github.com/andrebriggs/gen3-investigation).

## Initial Setup

- Ran `git clone https://github.com/uc-cdis/compose-services.git`
- Navigated to `$ cd compose-services`
- Ran `$ bash ./creds_setup.sh`
- Updated `./secrets/user.yaml`: entered my gmail account in the appropriate places
- Updated `./secrets/fence-config.yaml`: added Google API credentials
- Changed the `docker-compose.yml` file by removing some unwanted services. (File is located in repo.)
- Ran `docker-compose up`.

## Program and Project Creation

- Created a Program. (See `./submissions/program.json`)
- Created a Project. (See `./submissions/project.json`)

## Update Permissions

- Updated `./secrets/user.yaml`: made changes to `authz.resources` and `authz.policies`
- Ran `docker exec -it fence-service fence-create sync --arborist http://arborist-service --yaml user.yaml` to propigate updates.

## Uploaded Data

- Created an Experiment. (See `./submissions/experiment.json`)
- Created a Case. (See `./submissions/case.json`)

## Changed an Existing Node

- Brought down all of the services.
- Changed the `docker-compose.yml` file: told Gen3 to use the local data dictionary instead of the one on Amazon S3.
- Modified the `case.yaml` file in the local data dictionary. (Added `disease_type` to the `required` list.) (See `./dictionaryfiles/case.yaml`.)
- Brought everything back up again and made another Case submission. (See `./submissions/case2.json`.)

  - As far as I could tell, the changes I made to the data dictionary were enforced.
    - I tried to submit a case without providing an entry for `disease_type`, but was not allowed.
      - I could tell that the changes were being enforced in the browser because of the nature of the form submission.
      - I could also tell that the changes were being enforced on the backend because I manually editted the JSON file before submitting, but I got a 400 response when I removed `disease_type` from the JSON.

## Added a Node

- Brought down all of the services and created a file called `subject.yaml`. (See `./dictionaryfiles/subject.yaml`.)
- Brought everything back up again and submitted an entry to the subject node. (See `./submissions/subject.json`.)
- Verified that the submission was being stored on the backend:

## Verification

Logged on to the database

```
docker exec -it compose-services_postgres_1 psql -h localhost -d metadata_db -U fence_user
```

Looked for tables and saw a table called `node_subject`

```
\dt
```

Checked the contents of the table and saw that my data was stored there.

```
select * from node_subject;
```

Left the database:

```
\q
```
