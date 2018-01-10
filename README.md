# conga-aem-smoke-test

This role runs smoke tests on AEM author and AEM publish instances based
upon a CONGA configuration. The tests use the build in test
functionality of the replication agents.

The following tests are executed:
* publish test (Author)
* flush test (Author and Publish)

## Requirements

This role requires Ansible 2.0 or higher and works with AEM 6.0 or
higher.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    aemst_publish_test_pattern: "succeeded"

The text to search for on the publish test result page

    aemst_flush_test_pattern: "succeeded"

The text to search for on the flush test result page

    aemst_user: admin

The username to use
    
    aemst_password: admin

The password to use

## Dependencies

This role depends on the
[conga-facts](https://github.com/wcm-io-devops/ansible-conga-facts) role
for accessing the CONGA configuration model.

## Compiles the CONGA configuration and runs a smoke test for author and publish

    - hosts: localhost
	  roles:
	    - conga-maven
	
	# run smoke tests for aem-author
    - hosts: "&{{ conga_environment }}:aem-author"
      environment: "{{ proxy_env | default({}) }}"
      vars:
        conga_node: aem-author
        conga_role_mapping: aem-cms
    
      roles:
        - conga-aem-smoke-test
    
    # run smoke tests for aem-publish
    - hosts: "&{{ conga_environment }}:aem-publish"
      environment: "{{ proxy_env | default({}) }}"
      vars:
        conga_node: aem-publish
        conga_role_mapping: aem-cms
    
      roles:
        - conga-aem-smoke-test
