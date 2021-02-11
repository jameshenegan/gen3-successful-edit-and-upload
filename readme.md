# Successful Node Creation and Data Upload

For the initial setup, I mainly followed [this readme by Andre Briggs](https://github.com/andrebriggs/gen3-investigation).

### Setup

1. Run `git clone https://github.com/uc-cdis/compose-services.git`
2. Navigate to `$ cd compose-services`
3. Run `$ bash ./creds_setup.sh`
4. Open in VS Code `$ code .`
5. Updated `./secrets/user.yaml`: entered my gmail account in the appropriate places
6. Updated `./secrets/fence-config.yaml`: added Google API credentials
7. Changed the `docker-compose.yml` file by removing some unwanted services. (File is located in repo.)
8. Ran `docker-compose up`.
9. Created a Program. (See `./submissions/program.json`)
10. Created a Project. (See `./submissions/project.json`)
11. Updated `./secrets/user.yaml`: made changes to `authz.resources` and `authz.policies`
12. Ran `docker exec -it fence-service fence-create sync --arborist http://arborist-service --yaml user.yaml` to propigate updates.
13. Created an Experiment. (See `./submissions/experiment.json`)
14. Created a Case. (See `./submissions/case.json`)
15. Brought down all of the services.
16. Changed the `docker-compose.yml` file: told Gen3 to use the local data dictionary instead of the one on Amazon S3.
17. Modified the `case.yaml` file in the local data dictionary. (Added `disease_type` to the `required` list.) (See `./dictionaryfiles/case.yaml`.)
18. Brought everything back up again and made another Case submission. (See `./submissions/case2.json`.)

- As far as I could tell, the changes I made to the data dictionary were enforced.
- I tried to submit a case without providing an entry for `disease_type`, but was not allowed.
- I could tell that the changes were being enforced in the browser because of the nature of the form submission.
- I could also tell that the changes were being enforced on the backend because I manually editted the JSON file before submitting, but I got a 400 response when I removed `disease_type` from the JSON.

19. Brought down all of the services and created a file called `subject.yaml`. (See `./dictionaryfiles/subject.yaml`.)
20. Brought everything back up again and submitted an entry to the subject node. (See `./submissions/subject.json`.)
21. Verified that the submission was being stored on the backend:

```
docker exec -it compose-services_postgres_1 psql -h localhost -d metadata_db -U fence_user
\dt
```

(Saw a table called `node_subject`)

```
select * from node_subject;
```

(Saw my entry)

```
\q
```
