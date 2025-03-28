
#### Dulux UI:
1. delete conformation todo
2. Add history in the view panel
3. Change the colors to more contrasting and appealing colors
4. add a csv uploader
5. Add filters and sorting across columns
#### Backend api fixes:
1. add types support to where and order by
#### Not Immediate

1. Allow bulk csv uploads for create and update (the logic can be based on time window)
2. Make disposition changes and keep track on the history of disposition changes
3. Make configurable disposition qualification rules on the basis of which we can change the status of the lead.
4. Allow the disposition changes trigger workflows based on the disposition and the workflows should be configurable
5. integrations i.e., tata dialer, gala box(WABA), emails, etc.,  

### Ottopilot :
1. normalize tables
2. add arrays
3. schema updation
	1. rename fields
	2. delete fields
	3. add fields
	4. change type of fields
4. Foreign keys
5. Read
	1. Shift everything to requestly
	2. getCte usage
6. UI
7. add slug in projects
8. RBAC
9. API structure
10. Graph-QL
	1. custom implementation
	2. real implementation
11. Janus integration
12. custom types
13. Transactions
14. The storage DB's can be multiple i.e.,  will be horizontally scaled and completely dynamic, So the system DB should store all of these and our Ottopilot application should be able to handle this while creating of projects.

