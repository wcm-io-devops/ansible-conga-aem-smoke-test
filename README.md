# wcm_io_devops.conga_aem_smoke_test

This role runs smoke tests on AEM author and AEM publish instances based
upon a CONGA configuration. The tests use the build in test
functionality of the replication agents.

The following tests are executed:
* publish test (Author)
* flush test (Author and Publish)

> This role was developed as part of the
> [wcm.io DevOps Ansible Automation for AEM](http://devops.wcm.io/ansible-aem/)
> to integrate Ansible with
> [CONGA](http://devops.wcm.io/conga/).

## Requirements

This role requires Ansible 2.7 or higher and works with AEM 6.0 or
higher.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    conga_aemst_publish_test_pattern: "succeeded"

The text to search for on the publish test result page.

    conga_aemst_publish_test_timeout: 60

The timeout for the publish test.

    conga_aemst_flush_test_pattern: "succeeded"

The text to search for on the flush test result page.

    conga_aemst_flush_test_timeout: 60

The timeout for the flush test.

    conga_aemst_user: "{{ conga_config.quickstart.adminUser.username | default('admin')}}"

The username to use.
    
    conga_aemst_password: "{{ conga_config.quickstart.adminUser.password | default('admin')}}"

The password to use.

## Dependencies

This role depends on the
[wcm_io_devops.conga_facts](https://github.com/wcm-io-devops/ansible-conga-facts) role
for accessing the CONGA configuration model.

## Compiles the CONGA configuration and runs a smoke test for author and publish

    - hosts: localhost
	  roles:
	    - wcm_io_devops.conga_maven
	
	# run smoke tests for aem-author
    - hosts: "&{{ conga_environment }}:aem-author"
      environment: "{{ proxy_env | default({}) }}"
      vars:
        conga_node: aem-author
        conga_role_mapping: aem-cms
    
      roles:
        - wcm_io_devops.conga_aem_smoke_test
    
    # run smoke tests for aem-publish
    - hosts: "&{{ conga_environment }}:aem-publish"
      environment: "{{ proxy_env | default({}) }}"
      vars:
        conga_node: aem-publish
        conga_role_mapping: aem-cms
    
      roles:
        - wcm_io_devops.conga_aem_smoke_test
