credential-gcp.json
```
{
  "type": "service_account",
  "project_id": "plasma-winter-426302-q4",
  "private_key_id": "21b40efba2404a81e055427cb9eef69a2122b10d",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQC2FR2jhx9237MF\nAWtR1QdINAeZ/CZ/xmjQlhx+6NyVf662hWnkmpahWBjQWQ1jCt7kKcRmKbqhZPgh\n4q/noVjaGVAbzXDPJJN6+vh3lvKi4xOrG8RMvu2vfD2xNCHnkgih3yfWHWDWAKsg\nRohyJAhgRglqkEIGA00cBBym3sUQf12TFCQNF3t/RX9vxr+2AWgWzx6CxDPNh+Jn\nfqDZnIVgioHB+vsUaMKgI4uUtZfTkabjtieoSqZkU9TsASPGdG5DSm+J+/iF50h2\nVqTiMtW7Q6vyJjQr41utn6OQoRa2yLqZF7SKA04FHWRf9t5uTiXlDchBkj5ojmiD\nGVl7bCtFAgMBAAECggEAKygvGbrluMnFxzpyYveAndsDMDrFL0TqRAJIZ8YuvlqA\noS9XDYGckUptuzhYRXDmqqLBs8tROn7Rl0qBEgA6rJsSUzyq79YGBMCmksXa5cO3\nvjc7HEumz5C9mJo4LQh+dkuLyCK3eJG4/dHp9k/XEmaXRcRCeeVfafQJjH3BrKp0\nWWYbUEB0zNAmVtE1eMer2dsbGGhXm8k0yIdOYhBmtFlQGUrgWYBUUuBaHvUYI9mf\nV8zSYNbg+YojTjBx7Mg2vxIFLWbdcFZKEGcpYvvNsrZbEFQXWoObPS948xr79mNi\n9Fp3QYVRLFIi0NJHn/txeTdy0x2K5v+DnihznOxj8QKBgQD2T2V6OAJptPFHJQ1S\nUHNHC0SPKq/TDYTU87wTUfol4KiPqtTlGFY4nOhoRIbOrunco/T5m9MLOCUIlwRm\ny1cpoyYf2i3IDdX8myT/daZr3Ow/7Abzbi7MPBDbt2yfQRF1WtonEyeQ81BqIHl8\nKEI1FteD4slIK72/b0VMgTjhuQKBgQC9PuECA37GTh/b2m7/IDPt2RIFtPTgCxvB\ndOmBiL30/XjXklD3QCJMH/p10Vp7cEFkuN3+PHAPpmJJLLijjDkR+exbGcyTraiO\nWhcTKDeX9tjdE8HUgwTVLRHKTYJNj2t0WUUQ3zH8cWx7XYHonlCsYDSzY+EyN7ty\nuM4ppbJL7QKBgAi89leopwVDAxBIEznpWr2Ze7wsgoJVR3Ial4CD9wDjAHfgUp8y\nBtUJVAFm9PVeJTPLqUQ1r/4E5uNwIBrZeeUjQZX9soQXYZENm/loHhhThRobcH+w\nV/6s3tg8oKDhuRHVwEmEl3HAAAlTz5uE/hxODCVEpWlnC9s/wlCdgPwZAoGBAIn8\n0UAQoF2kFWLPUOPB7VteTd/PZEYAk4pp3uFOfYOnVneI/nRqVRfAsXU644jh/yyc\nB3IbS3J91WiZrT/DPNG4s/hxRVPg6ehyFCUpy++IU/RPNaPorJtrs28ZOQuoqac6\neDunIuF5KqqBMfoVhalKtOKgz1E4hftOeTSw1uK5AoGAG62gBiKVLCiLL8eAjXDD\nzvTwFN1WM+dN+HKYzr02cK/KYM8jxllLK3H37dvHpKT/srsavum4JGlTKxmr+whW\nftj5C342A9bfl3PXQftAgsqpS2mtRTdI8tnfQxqXLGmV38haW0hk5g28EkVDXYI6\nCacW76+NX/iRxQ1o6JpEuNs=\n-----END PRIVATE KEY-----\n",
  "client_email": "gcp-terraform@plasma-winter-426302-q4.iam.gserviceaccount.com",
  "client_id": "109984128499331149702",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/gcp-terraform%40plasma-winter-426302-q4.iam.gserviceaccount.com",
  "universe_domain": "googleapis.com"
}
```
main.tf
```

provider "google" {
    project = "plasma-winter-426302-q4"
    credentials = "${file("credential-gcp.json")}"
    region = "asia-southeast2"
    zone = "asia-southeast2-a"
}

resource "google_compute_instance" "rancher-cplane" {
    name = "rancher"
    machine_type = "e2-medium"
    zone = "asia-southeast2-a"

    boot_disk {
        initialize_params {
        image = "debian-cloud/debian-12"
        size = "30"
        type = "pd-ssd"
        }
    }
    network_interface {
        network = "default"
        access_config {
        }
    }
}

resource "google_compute_instance" "rancher-worker" {
    name = "node-2"
    machine_type = "e2-standard-2"
    zone = "asia-southeast2-a"

    boot_disk {
        initialize_params {
        image = "debian-cloud/debian-12"
        size = "30"
        type = "pd-ssd"
        }
    }
    network_interface {
        network = "default"
        access_config {
        }
    }
}

resource "google_compute_instance" "nginx" {
    name = "admin"
    machine_type = "e2-medium"
    zone = "asia-southeast2-a"

    boot_disk {
        initialize_params {
        image = "debian-cloud/debian-12"
        size = "15"
        type = "pd-ssd"
        }
    }
    network_interface {
        network = "default"
        access_config {
        }
    }
}
```
terraform.tfstate
```
{
  "version": 4,
  "terraform_version": "1.8.3",
  "serial": 17,
  "lineage": "8090ffe7-72c8-0a11-81bf-59e6b814cc49",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "google_compute_instance",
      "name": "rancher-cplane",
      "provider": "provider[\"registry.terraform.io/hashicorp/google\"]",
      "instances": [
        {
          "schema_version": 6,
          "attributes": {
            "advanced_machine_features": [],
            "allow_stopping_for_update": null,
            "attached_disk": [],
            "boot_disk": [
              {
                "auto_delete": true,
                "device_name": "persistent-disk-0",
                "disk_encryption_key_raw": "",
                "disk_encryption_key_sha256": "",
                "initialize_params": [
                  {
                    "enable_confidential_compute": false,
                    "image": "https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-12-bookworm-v20240515",
                    "labels": {},
                    "provisioned_iops": 0,
                    "provisioned_throughput": 0,
                    "resource_manager_tags": {},
                    "size": 30,
                    "type": "pd-ssd"
                  }
                ],
                "kms_key_self_link": "",
                "mode": "READ_WRITE",
                "source": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/disks/nginx-proxy-manager"
              }
            ],
            "can_ip_forward": false,
            "confidential_instance_config": [],
            "cpu_platform": "Intel Broadwell",
            "current_status": "RUNNING",
            "deletion_protection": false,
            "description": "",
            "desired_status": null,
            "effective_labels": {},
            "enable_display": false,
            "guest_accelerator": [],
            "hostname": "",
            "id": "projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/nginx-proxy-manager",
            "instance_id": "5595722454208811617",
            "label_fingerprint": "42WmSpB8rSM=",
            "labels": {},
            "machine_type": "e2-medium",
            "metadata": {},
            "metadata_fingerprint": "9sjup__zU4E=",
            "metadata_startup_script": null,
            "min_cpu_platform": "",
            "name": "nginx-proxy-manager",
            "network_interface": [
              {
                "access_config": [
                  {
                    "nat_ip": "34.101.144.252",
                    "network_tier": "PREMIUM",
                    "public_ptr_domain_name": ""
                  }
                ],
                "alias_ip_range": [],
                "internal_ipv6_prefix_length": 0,
                "ipv6_access_config": [],
                "ipv6_access_type": "",
                "ipv6_address": "",
                "name": "nic0",
                "network": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/global/networks/default",
                "network_ip": "10.184.0.14",
                "nic_type": "",
                "queue_count": 0,
                "stack_type": "IPV4_ONLY",
                "subnetwork": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/regions/asia-southeast2/subnetworks/default",
                "subnetwork_project": "project-kecil-kecilan"
              }
            ],
            "network_performance_config": [],
            "params": [],
            "project": "project-kecil-kecilan",
            "reservation_affinity": [],
            "resource_policies": [],
            "scheduling": [
              {
                "automatic_restart": true,
                "instance_termination_action": "",
                "local_ssd_recovery_timeout": [],
                "min_node_cpus": 0,
                "node_affinities": [],
                "on_host_maintenance": "MIGRATE",
                "preemptible": false,
                "provisioning_model": "STANDARD"
              }
            ],
            "scratch_disk": [],
            "self_link": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/nginx-proxy-manager",
            "service_account": [],
            "shielded_instance_config": [
              {
                "enable_integrity_monitoring": true,
                "enable_secure_boot": false,
                "enable_vtpm": true
              }
            ],
            "tags": [],
            "tags_fingerprint": "42WmSpB8rSM=",
            "terraform_labels": {},
            "timeouts": null,
            "zone": "asia-southeast2-a"
          },
          "sensitive_attributes": [
            [
              {
                "type": "get_attr",
                "value": "boot_disk"
              },
              {
                "type": "index",
                "value": {
                  "value": 0,
                  "type": "number"
                }
              },
              {
                "type": "get_attr",
                "value": "disk_encryption_key_raw"
              }
            ]
          ],
          "private": "eyJlMmJmYjczMC1lY2FhLTExZTYtOGY4OC0zNDM2M2JjN2M0YzAiOnsiY3JlYXRlIjoxMjAwMDAwMDAwMDAwLCJkZWxldGUiOjEyMDAwMDAwMDAwMDAsInVwZGF0ZSI6MTIwMDAwMDAwMDAwMH0sInNjaGVtYV92ZXJzaW9uIjoiNiJ9"
        }
      ]
    },
    {
      "mode": "managed",
      "type": "google_compute_instance",
      "name": "rancher-worker",
      "provider": "provider[\"registry.terraform.io/hashicorp/google\"]",
      "instances": [
        {
          "schema_version": 6,
          "attributes": {
            "advanced_machine_features": [],
            "allow_stopping_for_update": null,
            "attached_disk": [],
            "boot_disk": [
              {
                "auto_delete": true,
                "device_name": "persistent-disk-0",
                "disk_encryption_key_raw": "",
                "disk_encryption_key_sha256": "",
                "initialize_params": [
                  {
                    "enable_confidential_compute": false,
                    "image": "https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-12-bookworm-v20240515",
                    "labels": {},
                    "provisioned_iops": 0,
                    "provisioned_throughput": 0,
                    "resource_manager_tags": null,
                    "size": 30,
                    "type": "pd-ssd"
                  }
                ],
                "kms_key_self_link": "",
                "mode": "READ_WRITE",
                "source": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/disks/frontend"
              }
            ],
            "can_ip_forward": false,
            "confidential_instance_config": [],
            "cpu_platform": "Intel Broadwell",
            "current_status": "RUNNING",
            "deletion_protection": false,
            "description": "",
            "desired_status": null,
            "effective_labels": {},
            "enable_display": false,
            "guest_accelerator": [],
            "hostname": "",
            "id": "projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/frontend",
            "instance_id": "1244259188461997713",
            "label_fingerprint": "42WmSpB8rSM=",
            "labels": null,
            "machine_type": "e2-standard-2",
            "metadata": null,
            "metadata_fingerprint": "9sjup__zU4E=",
            "metadata_startup_script": null,
            "min_cpu_platform": "",
            "name": "frontend",
            "network_interface": [
              {
                "access_config": [
                  {
                    "nat_ip": "34.101.190.242",
                    "network_tier": "PREMIUM",
                    "public_ptr_domain_name": ""
                  }
                ],
                "alias_ip_range": [],
                "internal_ipv6_prefix_length": 0,
                "ipv6_access_config": [],
                "ipv6_access_type": "",
                "ipv6_address": "",
                "name": "nic0",
                "network": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/global/networks/default",
                "network_ip": "10.184.0.15",
                "nic_type": "",
                "queue_count": 0,
                "stack_type": "IPV4_ONLY",
                "subnetwork": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/regions/asia-southeast2/subnetworks/default",
                "subnetwork_project": "project-kecil-kecilan"
              }
            ],
            "network_performance_config": [],
            "params": [],
            "project": "project-kecil-kecilan",
            "reservation_affinity": [],
            "resource_policies": null,
            "scheduling": [
              {
                "automatic_restart": true,
                "instance_termination_action": "",
                "local_ssd_recovery_timeout": [],
                "min_node_cpus": 0,
                "node_affinities": [],
                "on_host_maintenance": "MIGRATE",
                "preemptible": false,
                "provisioning_model": "STANDARD"
              }
            ],
            "scratch_disk": [],
            "self_link": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/frontend",
            "service_account": [],
            "shielded_instance_config": [
              {
                "enable_integrity_monitoring": true,
                "enable_secure_boot": false,
                "enable_vtpm": true
              }
            ],
            "tags": null,
            "tags_fingerprint": "42WmSpB8rSM=",
            "terraform_labels": {},
            "timeouts": null,
            "zone": "asia-southeast2-a"
          },
          "sensitive_attributes": [
            [
              {
                "type": "get_attr",
                "value": "boot_disk"
              },
              {
                "type": "index",
                "value": {
                  "value": 0,
                  "type": "number"
                }
              },
              {
                "type": "get_attr",
                "value": "disk_encryption_key_raw"
              }
            ]
          ],
          "private": "eyJlMmJmYjczMC1lY2FhLTExZTYtOGY4OC0zNDM2M2JjN2M0YzAiOnsiY3JlYXRlIjoxMjAwMDAwMDAwMDAwLCJkZWxldGUiOjEyMDAwMDAwMDAwMDAsInVwZGF0ZSI6MTIwMDAwMDAwMDAwMH0sInNjaGVtYV92ZXJzaW9uIjoiNiJ9"
        }
      ]
    }
  ],
  "check_results": null
}
```
terraform.tfstate.backup
```
{
  "version": 4,
  "terraform_version": "1.8.3",
  "serial": 15,
  "lineage": "8090ffe7-72c8-0a11-81bf-59e6b814cc49",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "google_compute_instance",
      "name": "rancher-cplane",
      "provider": "provider[\"registry.terraform.io/hashicorp/google\"]",
      "instances": [
        {
          "schema_version": 6,
          "attributes": {
            "advanced_machine_features": [],
            "allow_stopping_for_update": null,
            "attached_disk": [],
            "boot_disk": [
              {
                "auto_delete": true,
                "device_name": "persistent-disk-0",
                "disk_encryption_key_raw": "",
                "disk_encryption_key_sha256": "",
                "initialize_params": [
                  {
                    "enable_confidential_compute": false,
                    "image": "https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-12-bookworm-v20240515",
                    "labels": {},
                    "provisioned_iops": 0,
                    "provisioned_throughput": 0,
                    "resource_manager_tags": null,
                    "size": 30,
                    "type": "pd-ssd"
                  }
                ],
                "kms_key_self_link": "",
                "mode": "READ_WRITE",
                "source": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/disks/nginx-proxy-manager"
              }
            ],
            "can_ip_forward": false,
            "confidential_instance_config": [],
            "cpu_platform": "Intel Broadwell",
            "current_status": "RUNNING",
            "deletion_protection": false,
            "description": "",
            "desired_status": null,
            "effective_labels": {},
            "enable_display": false,
            "guest_accelerator": [],
            "hostname": "",
            "id": "projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/nginx-proxy-manager",
            "instance_id": "5595722454208811617",
            "label_fingerprint": "42WmSpB8rSM=",
            "labels": null,
            "machine_type": "e2-medium",
            "metadata": null,
            "metadata_fingerprint": "9sjup__zU4E=",
            "metadata_startup_script": null,
            "min_cpu_platform": "",
            "name": "nginx-proxy-manager",
            "network_interface": [
              {
                "access_config": [
                  {
                    "nat_ip": "34.101.144.252",
                    "network_tier": "PREMIUM",
                    "public_ptr_domain_name": ""
                  }
                ],
                "alias_ip_range": [],
                "internal_ipv6_prefix_length": 0,
                "ipv6_access_config": [],
                "ipv6_access_type": "",
                "ipv6_address": "",
                "name": "nic0",
                "network": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/global/networks/default",
                "network_ip": "10.184.0.14",
                "nic_type": "",
                "queue_count": 0,
                "stack_type": "IPV4_ONLY",
                "subnetwork": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/regions/asia-southeast2/subnetworks/default",
                "subnetwork_project": "project-kecil-kecilan"
              }
            ],
            "network_performance_config": [],
            "params": [],
            "project": "project-kecil-kecilan",
            "reservation_affinity": [],
            "resource_policies": null,
            "scheduling": [
              {
                "automatic_restart": true,
                "instance_termination_action": "",
                "local_ssd_recovery_timeout": [],
                "min_node_cpus": 0,
                "node_affinities": [],
                "on_host_maintenance": "MIGRATE",
                "preemptible": false,
                "provisioning_model": "STANDARD"
              }
            ],
            "scratch_disk": [],
            "self_link": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/nginx-proxy-manager",
            "service_account": [],
            "shielded_instance_config": [
              {
                "enable_integrity_monitoring": true,
                "enable_secure_boot": false,
                "enable_vtpm": true
              }
            ],
            "tags": null,
            "tags_fingerprint": "42WmSpB8rSM=",
            "terraform_labels": {},
            "timeouts": null,
            "zone": "asia-southeast2-a"
          },
          "sensitive_attributes": [
            [
              {
                "type": "get_attr",
                "value": "boot_disk"
              },
              {
                "type": "index",
                "value": {
                  "value": 0,
                  "type": "number"
                }
              },
              {
                "type": "get_attr",
                "value": "disk_encryption_key_raw"
              }
            ]
          ],
          "private": "eyJlMmJmYjczMC1lY2FhLTExZTYtOGY4OC0zNDM2M2JjN2M0YzAiOnsiY3JlYXRlIjoxMjAwMDAwMDAwMDAwLCJkZWxldGUiOjEyMDAwMDAwMDAwMDAsInVwZGF0ZSI6MTIwMDAwMDAwMDAwMH0sInNjaGVtYV92ZXJzaW9uIjoiNiJ9"
        }
      ]
    }
  ],
  "check_results": null
}
bhq@LAPTOP-195K8LCM:~/terraform/gcp$ ls
credential-gcp.json  main.tf  terraform.tfstate  terraform.tfstate.backup
bhq@LAPTOP-195K8LCM:~/terraform/gcp$ cat terraform.tfstate.backup
{
  "version": 4,
  "terraform_version": "1.8.3",
  "serial": 15,
  "lineage": "8090ffe7-72c8-0a11-81bf-59e6b814cc49",
  "outputs": {},
  "resources": [
    {
      "mode": "managed",
      "type": "google_compute_instance",
      "name": "rancher-cplane",
      "provider": "provider[\"registry.terraform.io/hashicorp/google\"]",
      "instances": [
        {
          "schema_version": 6,
          "attributes": {
            "advanced_machine_features": [],
            "allow_stopping_for_update": null,
            "attached_disk": [],
            "boot_disk": [
              {
                "auto_delete": true,
                "device_name": "persistent-disk-0",
                "disk_encryption_key_raw": "",
                "disk_encryption_key_sha256": "",
                "initialize_params": [
                  {
                    "enable_confidential_compute": false,
                    "image": "https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-12-bookworm-v20240515",
                    "labels": {},
                    "provisioned_iops": 0,
                    "provisioned_throughput": 0,
                    "resource_manager_tags": null,
                    "size": 30,
                    "type": "pd-ssd"
                  }
                ],
                "kms_key_self_link": "",
                "mode": "READ_WRITE",
                "source": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/disks/nginx-proxy-manager"
              }
            ],
            "can_ip_forward": false,
            "confidential_instance_config": [],
            "cpu_platform": "Intel Broadwell",
            "current_status": "RUNNING",
            "deletion_protection": false,
            "description": "",
            "desired_status": null,
            "effective_labels": {},
            "enable_display": false,
            "guest_accelerator": [],
            "hostname": "",
            "id": "projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/nginx-proxy-manager",
            "instance_id": "5595722454208811617",
            "label_fingerprint": "42WmSpB8rSM=",
            "labels": null,
            "machine_type": "e2-medium",
            "metadata": null,
            "metadata_fingerprint": "9sjup__zU4E=",
            "metadata_startup_script": null,
            "min_cpu_platform": "",
            "name": "nginx-proxy-manager",
            "network_interface": [
              {
                "access_config": [
                  {
                    "nat_ip": "34.101.144.252",
                    "network_tier": "PREMIUM",
                    "public_ptr_domain_name": ""
                  }
                ],
                "alias_ip_range": [],
                "internal_ipv6_prefix_length": 0,
                "ipv6_access_config": [],
                "ipv6_access_type": "",
                "ipv6_address": "",
                "name": "nic0",
                "network": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/global/networks/default",
                "network_ip": "10.184.0.14",
                "nic_type": "",
                "queue_count": 0,
                "stack_type": "IPV4_ONLY",
                "subnetwork": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/regions/asia-southeast2/subnetworks/default",
                "subnetwork_project": "project-kecil-kecilan"
              }
            ],
            "network_performance_config": [],
            "params": [],
            "project": "project-kecil-kecilan",
            "reservation_affinity": [],
            "resource_policies": null,
            "scheduling": [
              {
                "automatic_restart": true,
                "instance_termination_action": "",
                "local_ssd_recovery_timeout": [],
                "min_node_cpus": 0,
                "node_affinities": [],
                "on_host_maintenance": "MIGRATE",
                "preemptible": false,
                "provisioning_model": "STANDARD"
              }
            ],
            "scratch_disk": [],
            "self_link": "https://www.googleapis.com/compute/v1/projects/project-kecil-kecilan/zones/asia-southeast2-a/instances/nginx-proxy-manager",
            "service_account": [],
            "shielded_instance_config": [
              {
                "enable_integrity_monitoring": true,
                "enable_secure_boot": false,
                "enable_vtpm": true
              }
            ],
            "tags": null,
            "tags_fingerprint": "42WmSpB8rSM=",
            "terraform_labels": {},
            "timeouts": null,
            "zone": "asia-southeast2-a"
          },
          "sensitive_attributes": [
            [
              {
                "type": "get_attr",
                "value": "boot_disk"
              },
              {
                "type": "index",
                "value": {
                  "value": 0,
                  "type": "number"
                }
              },
              {
                "type": "get_attr",
                "value": "disk_encryption_key_raw"
              }
            ]
          ],
          "private": "eyJlMmJmYjczMC1lY2FhLTExZTYtOGY4OC0zNDM2M2JjN2M0YzAiOnsiY3JlYXRlIjoxMjAwMDAwMDAwMDAwLCJkZWxldGUiOjEyMDAwMDAwMDAwMDAsInVwZGF0ZSI6MTIwMDAwMDAwMDAwMH0sInNjaGVtYV92ZXJzaW9uIjoiNiJ9"
        }
      ]
    }
  ],
  "check_results": null
}
```
