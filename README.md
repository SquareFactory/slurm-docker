# Docker Compose Slurm Controller + Slurm + REST API

This is a one node cluster.

## Configuration

### Setup NGINX Reverse Proxy

-  Fetch a private key `slurmrest.csquare.gcloud.key` and a chained certificate `slurmrest.csquare.gcloud.chained.crt` from a CA, and put them in the `certs` directory.

NGINX configuration is stored in the `nginx` folder.

Certificate request was :

```txt
Certificate Request:
    Data:
        Version: 1 (0x0)
        Subject: C = CH, ST = Switzerland, L = Lausanne, O = Cohesive Computing SA, OU = IT, CN = slurmrest.csquare.gcloud
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:9e:4e:4d:80:d3:4c:82:fe:24:f3:ab:86:e3:64:
                    34:5d:7f:de:f5:b3:91:1a:df:53:fb:41:33:ce:9d:
                    d1:c6:9b:1b:ac:ba:74:91:c2:96:ad:d9:4e:81:3a:
                    c5:98:72:b9:6c:d0:7e:a9:ec:60:14:3c:e3:71:7a:
                    0f:bc:96:3e:79:b9:28:a1:64:f4:be:70:93:31:80:
                    8d:cb:df:ae:1c:14:f0:7c:91:4e:0f:76:57:22:05:
                    64:9f:82:0c:50:2b:ff:3b:b7:a3:4e:f2:38:fa:06:
                    9d:86:a5:46:93:d3:d9:94:92:0b:cc:5a:84:c0:d3:
                    43:bc:b4:cc:47:64:04:f8:a8:11:6a:f0:b3:89:39:
                    8d:33:69:ba:fd:b4:50:53:50:33:82:03:d3:d0:8d:
                    4a:b9:aa:34:8c:93:ce:bb:cc:5d:fd:0f:f1:1d:cb:
                    9a:84:48:19:6d:f0:66:cd:d9:31:c9:6a:95:8e:2b:
                    64:57:6e:ab:45:6a:0f:18:d8:c7:90:52:50:a6:db:
                    02:26:e2:a1:7c:9d:10:fa:ca:5c:db:69:8c:c4:2f:
                    96:1f:fe:15:38:4f:08:88:5f:1d:ef:1b:4c:79:05:
                    d9:a5:49:5b:c6:99:e6:51:31:3c:89:23:ec:c1:85:
                    1b:98:9a:a3:7d:54:61:e7:fb:56:f5:d2:80:75:5b:
                    b8:65
                Exponent: 65537 (0x10001)
        Attributes:
        Requested Extensions:
            X509v3 Key Usage:
                Key Encipherment, Data Encipherment
            X509v3 Extended Key Usage:
                TLS Web Server Authentication
            X509v3 Subject Alternative Name:
                DNS:vpc.csquare.ai
    Signature Algorithm: sha256WithRSAEncryption
         00:0d:bd:23:61:c1:b0:b3:78:96:49:f0:2f:38:65:fe:b5:b3:
         73:9d:24:17:58:7a:d6:0f:02:6d:82:5e:d9:59:9a:1a:9a:4c:
         2f:df:43:08:0d:11:45:6a:bd:7d:fc:52:e9:5f:64:46:ce:72:
         25:e9:2e:c4:3f:8c:c5:14:3f:79:f4:2e:f4:a0:17:26:3c:48:
         32:01:0d:c9:97:47:fc:4f:62:a8:87:7f:29:38:00:fe:83:e4:
         e6:ba:24:ff:09:e7:13:84:e7:ad:44:c9:4b:e6:35:37:99:af:
         8c:b5:a9:2c:35:5b:34:68:99:d2:85:7c:63:c7:dd:46:28:f7:
         13:26:e4:ee:b4:1f:33:d2:aa:85:f5:1e:88:9f:f0:9f:65:cf:
         ad:9d:e0:3f:3c:50:8a:1c:aa:fa:d7:9b:0a:39:7e:18:87:b8:
         6b:be:61:f2:a8:dd:11:a3:3b:08:22:95:c7:7b:a9:17:0c:d4:
         51:36:aa:99:8f:23:be:54:13:df:6b:e0:a6:3e:9d:b8:ed:67:
         1c:48:00:83:f4:9f:38:cf:c9:da:b7:5e:a9:fb:10:66:ac:7c:
         19:a7:95:69:4d:5e:ae:c1:04:28:98:e7:9c:05:6c:a6:c3:58:
         88:02:a1:04:0e:06:83:37:72:4f:0e:35:2f:5e:50:08:63:73:
         9e:9b:b9:45
```

```txt
-----BEGIN CERTIFICATE REQUEST-----
MIIDGjCCAgICAQAwgYYxCzAJBgNVBAYTAkNIMRQwEgYDVQQIDAtTd2l0emVybGFu
ZDERMA8GA1UEBwwITGF1c2FubmUxHjAcBgNVBAoMFUNvaGVzaXZlIENvbXB1dGlu
ZyBTQTELMAkGA1UECwwCSVQxITAfBgNVBAMMGHNsdXJtcmVzdC5jc3F1YXJlLmdj
bG91ZDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAJ5OTYDTTIL+JPOr
huNkNF1/3vWzkRrfU/tBM86d0cabG6y6dJHClq3ZToE6xZhyuWzQfqnsYBQ843F6
D7yWPnm5KKFk9L5wkzGAjcvfrhwU8HyRTg92VyIFZJ+CDFAr/zu3o07yOPoGnYal
RpPT2ZSSC8xahMDTQ7y0zEdkBPioEWrws4k5jTNpuv20UFNQM4ID09CNSrmqNIyT
zrvMXf0P8R3LmoRIGW3wZs3ZMclqlY4rZFduq0VqDxjYx5BSUKbbAibioXydEPrK
XNtpjMQvlh/+FThPCIhfHe8bTHkF2aVJW8aZ5lExPIkj7MGFG5iao31UYef7VvXS
gHVbuGUCAwEAAaBOMEwGCSqGSIb3DQEJDjE/MD0wCwYDVR0PBAQDAgQwMBMGA1Ud
JQQMMAoGCCsGAQUFBwMBMBkGA1UdEQQSMBCCDnZwYy5jc3F1YXJlLmFpMA0GCSqG
SIb3DQEBCwUAA4IBAQAADb0jYcGws3iWSfAvOGX+tbNznSQXWHrWDwJtgl7ZWZoa
mkwv30MIDRFFar19/FLpX2RGznIl6S7EP4zFFD959C70oBcmPEgyAQ3Jl0f8T2Ko
h38pOAD+g+TmuiT/CecThOetRMlL5jU3ma+MtaksNVs0aJnShXxjx91GKPcTJuTu
tB8z0qqF9R6In/CfZc+tneA/PFCKHKr615sKOX4Yh7hrvmHyqN0RozsIIpXHe6kX
DNRRNqqZjyO+VBPfa+CmPp247WccSACD9J84z8nat16p+xBmrHwZp5VpTV6uwQQo
mOecBWymw1iIAqEEDgaDN3JPDjUvXlAIY3Oem7lF
-----END CERTIFICATE REQUEST-----
```

### Setup Slurm

-  Add the MUNGE key `munge.key` and the `jwt_hs256.key` to the `secrets` directory.

-  Configuration is stored in the `conf` folder. The `conf` folder is bound to `/etc/slurm`.

-  Bind the `ldap-certificate.pem` and the `sssd.conf` accordingly.

## Running

```sh
docker-compose up -d --build
```

## REST API

### Fetch an API Token

Fetch a token for the current logged user with:

```
scontrol token
```

Fetch a token for a specific user:

```
scontrol token username=<user>
```

By using an admin token from `root` or the `SlurmUser`, any request to the REST API is permitted under any account.

### Request

Given the environment variable `SLURM_JWT` set:

```sh
curl -H "X-SLURM-USER-TOKEN: $SLURM_JWT" -H "X-SLURM-USER-NAME: <user>" https://slurmrest.csquare.gcloud/openapi/v3
```

OpenAPI.json:

```json
{
  "tags": [
    {
      "name": "slurm",
      "description": "methods that query slurmctld"
    },
    {
      "name": "openapi",
      "description": "methods that query for OpenAPI specifications"
    }
  ],
  "paths": {
    "\/slurm\/v0.0.36\/diag": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_diag",
        "summary": "get diagnostics",
        "responses": {
          "200": {
            "description": "diagnostic results",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_diag"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_diag"
                }
              }
            }
          },
          "default": {
            "description": "unable to request ping test"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/ping": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_ping",
        "summary": "ping test",
        "responses": {
          "200": {
            "description": "results of ping test",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_pings"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_pings"
                }
              }
            }
          },
          "default": {
            "description": "unable to request ping test"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/jobs": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_get_jobs",
        "summary": "get list of jobs",
        "responses": {
          "200": {
            "description": "job(s) information",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_jobs_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_jobs_response"
                }
              }
            }
          },
          "default": {
            "description": "job not found"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/job\/{job_id}": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_get_job",
        "summary": "get job info",
        "parameters": [
          {
            "name": "job_id",
            "in": "path",
            "description": "Slurm Job ID",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "job(s) information",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_jobs_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_jobs_response"
                }
              }
            }
          },
          "default": {
            "description": "job not found"
          }
        }
      },
      "post": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_update_job",
        "summary": "update job",
        "parameters": [
          {
            "name": "job_id",
            "in": "path",
            "description": "Slurm Job ID",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "requestBody": {
          "description": "update job",
          "content": {
            "application\/json": {
              "schema": {
                "$ref": "#\/components\/schemas\/v0.0.36_job_properties"
              }
            },
            "application\/x-yaml": {
              "schema": {
                "$ref": "#\/components\/schemas\/v0.0.36_job_properties"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "job information"
          },
          "500": {
            "description": "job not found"
          }
        }
      },
      "delete": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_cancel_job",
        "summary": "cancel or signal job",
        "parameters": [
          {
            "name": "job_id",
            "in": "path",
            "description": "Slurm Job ID",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          },
          {
            "name": "signal",
            "in": "query",
            "description": "signal to send to job",
            "required": false,
            "style": "form",
            "explode": true,
            "schema": {
              "$ref": "#\/components\/schemas\/v0.0.36_signal"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "job cancelled or sent signal"
          },
          "500": {
            "description": "job not found"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/job\/submit": {
      "post": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_submit_job",
        "summary": "submit new job",
        "requestBody": {
          "description": "submit new job",
          "content": {
            "application\/json": {
              "schema": {
                "$ref": "#\/components\/schemas\/v0.0.36_job_submission"
              }
            },
            "application\/x-yaml": {
              "schema": {
                "$ref": "#\/components\/schemas\/v0.0.36_job_submission"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "job submitted",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_job_submission_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_job_submission_response"
                }
              }
            }
          },
          "default": {
            "description": "job rejected"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/nodes": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_get_nodes",
        "summary": "get all node info",
        "responses": {
          "200": {
            "description": "node information",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_nodes_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_nodes_response"
                }
              }
            }
          },
          "default": {
            "description": "no nodes in cluster"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/node\/{node_name}": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_get_node",
        "summary": "get node info",
        "parameters": [
          {
            "name": "node_name",
            "in": "path",
            "description": "Slurm Node Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "node information",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_nodes_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_nodes_response"
                }
              }
            }
          },
          "default": {
            "description": "node not found"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/partitions": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_get_partitions",
        "summary": "get all partition info",
        "responses": {
          "200": {
            "description": "partition information",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_partitions_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_partitions_response"
                }
              }
            }
          },
          "default": {
            "description": "no partitions found"
          }
        }
      }
    },
    "\/slurm\/v0.0.36\/partition\/{partition_name}": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmctld_get_partition",
        "summary": "get partition info",
        "parameters": [
          {
            "name": "partition_name",
            "in": "path",
            "description": "Slurm Partition Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "partition information",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_partitions_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/v0.0.36_partitions_response"
                }
              }
            }
          },
          "default": {
            "description": "no partitions found"
          }
        }
      }
    },
    "\/openapi.yaml": {
      "get": {
        "summary": "Retrieve OpenAPI Specification",
        "responses": {
          "200": {
            "description": "OpenAPI Specification"
          }
        }
      }
    },
    "\/openapi.json": {
      "get": {
        "summary": "Retrieve OpenAPI Specification",
        "responses": {
          "200": {
            "description": "OpenAPI Specification"
          }
        }
      }
    },
    "\/openapi": {
      "get": {
        "summary": "Retrieve OpenAPI Specification",
        "responses": {
          "200": {
            "description": "OpenAPI Specification"
          }
        }
      }
    },
    "\/openapi\/v3": {
      "get": {
        "summary": "Retrieve OpenAPI Specification",
        "responses": {
          "200": {
            "description": "OpenAPI Specification"
          }
        }
      }
    },
    "\/slurmdb\/v0.0.36\/job\/{job_id}": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_job",
        "summary": "Get job info",
        "parameters": [
          {
            "name": "job_id",
            "in": "path",
            "description": "Slurm Job ID",
            "required": true,
            "style": "label",
            "explode": false,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_job_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_job_info"
                }
              }
            },
            "description": "Job description"
          },
          "default": {
            "description": "Unable to find job"
          }
        }
      }
    },
    "\/slurmdb\/v0.0.36\/config": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_db_config",
        "summary": "Dump all configuration information",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_config_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_config_info"
                }
              }
            },
            "description": "slurmdbd configuration"
          },
          "default": {
            "description": "Unable to dump config"
          }
        }
      },
      "post": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_set_db_config",
        "summary": "Load all configuration information",
        "responses": {
          "200": {
            "description": "Load config",
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_config_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_config_response"
                }
              }
            }
          },
          "default": {
            "description": "Unable to set config"
          }
        }
      }
    },
    "\/slurmdb\/v0.0.36\/tres": {
      "post": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_update_tres",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_tres"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_tres"
                }
              }
            },
            "description": "List of TRES"
          },
          "default": {
            "description": "Unable to update TRES"
          }
        },
        "summary": "Set TRES info"
      },
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_tres",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_tres_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_tres_info"
                }
              }
            },
            "description": "List of TRES"
          },
          "default": {
            "description": "Unable to retrieve TRES"
          }
        },
        "summary": "Get TRES info"
      }
    },
    "\/slurmdb\/v0.0.36\/qos\/{qos_name}": {
      "delete": {
        "tags": [
          "slurm"
        ],
        "summary": "Delete QOS",
        "operationId": "slurmdbd_delete_qos",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_qos_delete"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_qos_delete"
                }
              }
            },
            "description": "Delete qos"
          },
          "default": {
            "description": "Unable to delete QOS"
          }
        },
        "parameters": [
          {
            "name": "qos_name",
            "in": "path",
            "description": "Slurm QOS Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      },
      "get": {
        "tags": [
          "slurm"
        ],
        "summary": "Get QOS info",
        "operationId": "slurmdbd_get_single_qos",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_qos_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_qos_info"
                }
              }
            },
            "description": "QOS information"
          },
          "default": {
            "description": "QOS not found"
          }
        },
        "parameters": [
          {
            "name": "qos_name",
            "in": "path",
            "description": "Slurm QOS Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      }
    },
    "\/slurmdb\/v0.0.36\/qos": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_qos",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_qos_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_qos_info"
                }
              }
            },
            "description": "List of QOS'"
          },
          "default": {
            "description": "QOS not found"
          }
        },
        "summary": "Get QOS list"
      }
    },
    "\/slurmdb\/v0.0.36\/associations": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_associations",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_associations_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_associations_info"
                }
              }
            },
            "description": "List of associations"
          },
          "default": {
            "description": "Association not found"
          }
        },
        "summary": "Get association list"
      }
    },
    "\/slurmdb\/v0.0.36\/association": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_association",
        "parameters": [
          {
            "name": "cluster",
            "in": "query",
            "description": "Cluster name",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "account",
            "in": "query",
            "description": "Account name",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "user",
            "in": "query",
            "description": "User name",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "partition",
            "in": "query",
            "description": "Partition Name",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_associations_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_associations_info"
                }
              }
            },
            "description": "List of associations"
          },
          "default": {
            "description": "Association not found"
          }
        },
        "summary": "Get association info"
      },
      "delete": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_delete_association",
        "parameters": [
          {
            "name": "cluster",
            "in": "query",
            "description": "Cluster name",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "account",
            "in": "query",
            "description": "Account name",
            "required": true,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "user",
            "in": "query",
            "description": "User name",
            "required": true,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "partition",
            "in": "query",
            "description": "Partition Name",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_association_delete"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_association_delete"
                }
              }
            },
            "description": "Delete associations"
          },
          "default": {
            "description": "Association not found or unable to delete association"
          }
        },
        "summary": "Delete association"
      }
    },
    "\/slurmdb\/v0.0.36\/user\/{user_name}": {
      "delete": {
        "tags": [
          "slurm"
        ],
        "summary": "Delete user",
        "operationId": "slurmdbd_delete_user",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_user_delete"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_user_delete"
                }
              }
            },
            "description": "Delete user"
          },
          "default": {
            "description": "User not found or unable to delete user"
          }
        },
        "parameters": [
          {
            "name": "user_name",
            "in": "path",
            "description": "Slurm User Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      },
      "get": {
        "tags": [
          "slurm"
        ],
        "summary": "Get user info",
        "operationId": "slurmdbd_get_user",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_user_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_user_info"
                }
              }
            },
            "description": "List of users"
          },
          "default": {
            "description": "User not found"
          }
        },
        "parameters": [
          {
            "name": "user_name",
            "in": "path",
            "description": "Slurm User Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      }
    },
    "\/slurmdb\/v0.0.36\/users": {
      "post": {
        "tags": [
          "slurm"
        ],
        "summary": "Update user",
        "operationId": "slurmdbd_update_users",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_user_update"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_user_update"
                }
              }
            },
            "description": "Update users"
          },
          "default": {
            "description": "User not found or not able to update user"
          }
        }
      },
      "get": {
        "tags": [
          "slurm"
        ],
        "summary": "Get user list",
        "operationId": "slurmdbd_get_users",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_user_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_user_info"
                }
              }
            },
            "description": "List of users"
          },
          "default": {
            "description": "User not found"
          }
        }
      }
    },
    "\/slurmdb\/v0.0.36\/cluster\/{cluster_name}": {
      "delete": {
        "tags": [
          "slurm"
        ],
        "summary": "Delete cluster",
        "operationId": "slurmdbd_delete_cluster",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_cluster_delete"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_cluster_delete"
                }
              }
            },
            "description": "Delete cluster"
          },
          "default": {
            "description": "Cluster not found or unable to delete cluster"
          }
        },
        "parameters": [
          {
            "name": "cluster_name",
            "in": "path",
            "description": "Slurm cluster name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      },
      "get": {
        "tags": [
          "slurm"
        ],
        "summary": "Get cluster info",
        "operationId": "slurmdbd_get_cluster",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_cluster_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_cluster_info"
                }
              }
            },
            "description": "Cluster information"
          },
          "default": {
            "description": "Cluster not found"
          }
        },
        "parameters": [
          {
            "name": "cluster_name",
            "in": "path",
            "description": "Slurm cluster name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      }
    },
    "\/slurmdb\/v0.0.36\/clusters": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_clusters",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_cluster_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_cluster_info"
                }
              }
            },
            "description": "List of clusters"
          },
          "default": {
            "description": "Cluster not found"
          }
        },
        "summary": "Get cluster list"
      },
      "post": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_add_clusters",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_cluster_add"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_cluster_add"
                }
              }
            },
            "description": "List of clusters"
          },
          "default": {
            "description": "Unable to add cluster"
          }
        },
        "summary": "Add clusters"
      }
    },
    "\/slurmdb\/v0.0.36\/wckey\/{wckey}": {
      "delete": {
        "tags": [
          "slurm"
        ],
        "summary": "Delete wckey",
        "operationId": "slurmdbd_delete_wckey",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_wckey_delete"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_wckey_delete"
                }
              }
            },
            "description": "Delete wckey"
          },
          "default": {
            "description": "wckey not found or unable to delete wckey"
          }
        },
        "parameters": [
          {
            "name": "wckey",
            "in": "path",
            "description": "Slurm wckey name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      },
      "get": {
        "tags": [
          "slurm"
        ],
        "summary": "Get wckey info",
        "operationId": "slurmdbd_get_wckey",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_wckey_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_wckey_info"
                }
              }
            },
            "description": "List of wckey"
          },
          "default": {
            "description": "wckey not found"
          }
        },
        "parameters": [
          {
            "name": "wckey",
            "in": "path",
            "description": "Slurm wckey name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      }
    },
    "\/slurmdb\/v0.0.36\/wckeys": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_wckeys",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_wckey_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_wckey_info"
                }
              }
            },
            "description": "List of wckeys"
          },
          "default": {
            "description": "wckey not found"
          }
        },
        "summary": "Get wckey list"
      },
      "post": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_add_wckeys",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_wckey_add"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_wckey_add"
                }
              }
            },
            "description": "List of wckeys"
          },
          "default": {
            "description": "Unable to add wckey"
          }
        },
        "summary": "Add wckeys"
      }
    },
    "\/slurmdb\/v0.0.36\/account\/{account_name}": {
      "delete": {
        "tags": [
          "slurm"
        ],
        "summary": "Delete account",
        "operationId": "slurmdbd_delete_account",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_account_delete"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_response_account_delete"
                }
              }
            },
            "description": "Delete account"
          },
          "default": {
            "description": "Unable to delete account"
          }
        },
        "parameters": [
          {
            "name": "account_name",
            "in": "path",
            "description": "Slurm Account Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      },
      "get": {
        "tags": [
          "slurm"
        ],
        "summary": "Get account info",
        "operationId": "slurmdbd_get_account",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_account_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_account_info"
                }
              }
            },
            "description": "List of accounts"
          },
          "default": {
            "description": "Account not found"
          }
        },
        "parameters": [
          {
            "name": "account_name",
            "in": "path",
            "description": "Slurm Account Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ]
      }
    },
    "\/slurmdb\/v0.0.36\/accounts": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_accounts",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_account_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_account_info"
                }
              }
            },
            "description": "List of accounts"
          },
          "default": {
            "description": "Account not found"
          }
        },
        "summary": "Get account list"
      },
      "post": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_update_account",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_account_response"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_account_response"
                }
              }
            },
            "description": "Add\/update list of accounts"
          },
          "default": {
            "description": "Unable to add or update accounts"
          }
        },
        "summary": "Update accounts"
      }
    },
    "\/slurmdb\/v0.0.36\/jobs": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_get_jobs",
        "parameters": [
          {
            "in": "query",
            "name": "submit_time",
            "description": "Filter by submission time\n Accepted formats:\n HH:MM[:SS] [AM|PM]\r\nMMDD[YY] or MM\/DD[\/YY] or MM.DD[.YY]\r\nMM\/DD[\/YY]-HH:MM[:SS]\r\nYYYY-MM-DD[THH:MM[:SS]]",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "start_time",
            "description": "Filter by start time\n Accepted formats:\n HH:MM[:SS] [AM|PM]\r\nMMDD[YY] or MM\/DD[\/YY] or MM.DD[.YY]\r\nMM\/DD[\/YY]-HH:MM[:SS]\r\nYYYY-MM-DD[THH:MM[:SS]]",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "end_time",
            "description": "Filter by end time\n Accepted formats:\n HH:MM[:SS] [AM|PM]\r\nMMDD[YY] or MM\/DD[\/YY] or MM.DD[.YY]\r\nMM\/DD[\/YY]-HH:MM[:SS]\r\nYYYY-MM-DD[THH:MM[:SS]]",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "account",
            "description": "Comma delimited list of accounts to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "association",
            "description": "Comma delimited list of associations to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "cluster",
            "description": "Comma delimited list of cluster to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "constraints",
            "description": "Comma delimited list of constraints to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "cpus_max",
            "description": "Number of CPUs high range",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "cpus_min",
            "description": "Number of CPUs low range",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "skip_steps",
            "description": "Report job step information",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "boolean"
            }
          },
          {
            "in": "query",
            "name": "disable_wait_for_result",
            "description": "Disable waiting for result from slurmdbd",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "boolean"
            }
          },
          {
            "in": "query",
            "name": "exit_code",
            "description": "Exit code of job",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "format",
            "description": "Comma delimited list of formats to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "group",
            "description": "Comma delimited list of groups to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "job_name",
            "description": "Comma delimited list of job names to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "nodes_max",
            "description": "Number of nodes high range",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "nodes_min",
            "description": "Number of nodes low range",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "partition",
            "description": "Comma delimited list of partitions to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "qos",
            "description": "Comma delimited list of QOS to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "reason",
            "description": "Comma delimited list of job reasons to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "reservation",
            "description": "Comma delimited list of reservations to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "state",
            "description": "Comma delimited list of states to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "step",
            "description": "Comma delimited list of job steps to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "node",
            "description": "Comma delimited list of used nodes to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "users",
            "description": "Comma delimited list of users to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          },
          {
            "in": "query",
            "name": "wckey",
            "description": "Comma delimited list of wckeys to match",
            "required": false,
            "style": "form",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_job_info"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_job_info"
                }
              }
            },
            "description": "List of jobs"
          },
          "default": {
            "description": "Unable to query jobs"
          }
        },
        "summary": "Get job list"
      }
    },
    "\/slurmdb\/v0.0.36\/diag": {
      "get": {
        "tags": [
          "slurm"
        ],
        "operationId": "slurmdbd_diag",
        "responses": {
          "200": {
            "content": {
              "application\/json": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_diag"
                }
              },
              "application\/x-yaml": {
                "schema": {
                  "$ref": "#\/components\/schemas\/dbv0.0.36_diag"
                }
              }
            },
            "description": "Dictionary of statistics"
          },
          "default": {
            "description": "Unable to query diagnostics"
          }
        },
        "summary": "Get slurmdb diagnostics"
      }
    },
    "\/slurm\/v0.0.35\/diag": {
      "get": {
        "summary": "get diagnostics",
        "responses": {
          "200": {
            "description": "dictionary of statistics"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/ping": {
      "get": {
        "summary": "ping test",
        "responses": {
          "200": {
            "description": "results of ping test"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/jobs": {
      "get": {
        "summary": "get list of jobs",
        "responses": {
          "200": {
            "description": "array of all job information in slurmctld"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/job\/{job_id}": {
      "get": {
        "summary": "get job info",
        "parameters": [
          {
            "name": "job_id",
            "in": "path",
            "description": "Slurm Job ID",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "job information"
          },
          "500": {
            "description": "job not found"
          }
        }
      },
      "post": {
        "summary": "update job",
        "parameters": [
          {
            "name": "job_id",
            "in": "path",
            "description": "Slurm Job ID",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          }
        ],
        "requestBody": {
          "description": "update job",
          "content": {
            "application\/json": {
              "schema": {
                "$ref": "#\/components\/schemas\/job_properties"
              }
            },
            "application\/x-yaml": {
              "schema": {
                "$ref": "#\/components\/schemas\/job_properties"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "job information"
          },
          "500": {
            "description": "job not found"
          }
        }
      },
      "delete": {
        "summary": "cancel or signal job",
        "parameters": [
          {
            "name": "job_id",
            "in": "path",
            "description": "Slurm Job ID",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "integer",
              "format": "int64"
            }
          },
          {
            "name": "signal",
            "in": "query",
            "description": "signal to send to job",
            "required": false,
            "style": "form",
            "explode": true,
            "schema": {
              "$ref": "#\/components\/schemas\/signal"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "job cancelled or sent signal"
          },
          "500": {
            "description": "job not found"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/job\/submit": {
      "post": {
        "summary": "submit new job",
        "requestBody": {
          "description": "submit new job",
          "content": {
            "application\/json": {
              "schema": {
                "$ref": "#\/components\/schemas\/job_properties"
              }
            },
            "application\/x-yaml": {
              "schema": {
                "$ref": "#\/components\/schemas\/job_properties"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "job submitted"
          },
          "500": {
            "description": "job rejected"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/nodes": {
      "get": {
        "summary": "get all node info",
        "responses": {
          "200": {
            "description": "nodes information"
          },
          "500": {
            "description": "no nodes in cluster"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/node\/{node_name}": {
      "get": {
        "summary": "get node info",
        "parameters": [
          {
            "name": "node_name",
            "in": "path",
            "description": "Slurm Node Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "node information"
          },
          "500": {
            "description": "node not found"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/partitions": {
      "get": {
        "summary": "get all partition info",
        "responses": {
          "200": {
            "description": "partitions information"
          },
          "500": {
            "description": "no partitions in cluster"
          }
        }
      }
    },
    "\/slurm\/v0.0.35\/partition\/{partition_name}": {
      "get": {
        "summary": "get partition info",
        "parameters": [
          {
            "name": "partition_name",
            "in": "path",
            "description": "Slurm Partition Name",
            "required": true,
            "style": "simple",
            "explode": false,
            "schema": {
              "type": "string"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "partition information"
          },
          "500": {
            "description": "partition not found"
          }
        }
      }
    }
  },
  "components": {
    "schemas": {
      "v0.0.36_diag": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_error"
            }
          },
          "statistics": {
            "type": "object",
            "description": "Slurm statistics",
            "properties": {
              "parts_packed": {
                "type": "integer",
                "description": "partition records packed"
              },
              "req_time": {
                "type": "integer",
                "description": "generation time"
              },
              "req_time_start": {
                "type": "integer",
                "description": "data since"
              },
              "server_thread_count": {
                "type": "integer",
                "description": "Server thread count"
              },
              "agent_queue_size": {
                "type": "integer",
                "description": "Agent queue size"
              },
              "agent_count": {
                "type": "integer",
                "description": "Agent count"
              },
              "agent_thread_count": {
                "type": "integer",
                "description": "Agent thread count"
              },
              "dbd_agent_queue_size": {
                "type": "integer",
                "description": "DBD Agent queue size"
              },
              "gettimeofday_latency": {
                "type": "integer",
                "description": "Latency for 1000 calls to gettimeofday()"
              },
              "schedule_cycle_max": {
                "type": "integer",
                "description": "Main Schedule max cycle"
              },
              "schedule_cycle_last": {
                "type": "integer",
                "description": "Main Schedule last cycle"
              },
              "schedule_cycle_total": {
                "type": "integer",
                "description": "Main Schedule cycle iterations"
              },
              "schedule_cycle_mean": {
                "type": "integer",
                "description": "Average time for Schedule Max cycle"
              },
              "schedule_cycle_mean_depth": {
                "type": "integer",
                "description": "Average depth for Schedule Max cycle"
              },
              "schedule_cycle_per_minute": {
                "type": "integer",
                "description": "Main Schedule Cycles per minute"
              },
              "schedule_queue_length": {
                "type": "integer",
                "description": "Main Schedule Last queue length"
              },
              "jobs_submitted": {
                "type": "integer",
                "description": "Job submitted"
              },
              "jobs_started": {
                "type": "integer",
                "description": "Job started"
              },
              "jobs_completed": {
                "type": "integer",
                "description": "Job completed"
              },
              "jobs_canceled": {
                "type": "integer",
                "description": "Job cancelled"
              },
              "jobs_failed": {
                "type": "integer",
                "description": "Job failed"
              },
              "jobs_pending": {
                "type": "integer",
                "description": "Job pending"
              },
              "jobs_running": {
                "type": "integer",
                "description": "Job running"
              },
              "job_states_ts": {
                "type": "integer",
                "description": "Job states timestamp"
              },
              "bf_backfilled_jobs": {
                "type": "integer",
                "description": "Total backfilled jobs (since last slurm start)"
              },
              "bf_last_backfilled_jobs": {
                "type": "integer",
                "description": "Total backfilled jobs (since last stats cycle start)"
              },
              "bf_backfilled_het_jobs": {
                "type": "integer",
                "description": "Total backfilled heterogeneous job components"
              },
              "bf_cycle_counter": {
                "type": "integer",
                "description": "Backfill Schedule Total cycles"
              },
              "bf_cycle_mean": {
                "type": "integer",
                "description": "Backfill Schedule Mean cycle"
              },
              "bf_cycle_max": {
                "type": "integer",
                "description": "Backfill Schedule Max cycle time"
              },
              "bf_last_depth": {
                "type": "integer",
                "description": "Backfill Schedule Last depth cycle"
              },
              "bf_last_depth_try": {
                "type": "integer",
                "description": "Backfill Schedule Mean cycle (try sched)"
              },
              "bf_depth_mean": {
                "type": "integer",
                "description": "Backfill Schedule Depth Mean"
              },
              "bf_depth_mean_try": {
                "type": "integer",
                "description": "Backfill Schedule Depth Mean (try sched)"
              },
              "bf_cycle_last": {
                "type": "integer",
                "description": "Backfill Schedule Last cycle time"
              },
              "bf_queue_len": {
                "type": "integer",
                "description": "Backfill Schedule Last queue length"
              },
              "bf_queue_len_mean": {
                "type": "integer",
                "description": "Backfill Schedule Mean queue length"
              },
              "bf_when_last_cycle": {
                "type": "integer",
                "description": "Last cycle timestamp"
              },
              "bf_active": {
                "type": "boolean",
                "description": "Backfill Schedule currently active"
              }
            }
          }
        }
      },
      "v0.0.36_pings": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_error"
            }
          },
          "pings": {
            "type": "array",
            "description": "slurm controller pings",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_ping"
            }
          }
        }
      },
      "v0.0.36_ping": {
        "type": "object",
        "properties": {
          "hostname": {
            "type": "string",
            "description": "slurm controller hostname"
          },
          "ping": {
            "type": "string",
            "description": "slurm controller host up",
            "enum": [
              "UP",
              "DOWN"
            ]
          },
          "mode": {
            "type": "string",
            "description": "slurm controller mode"
          },
          "status": {
            "type": "integer",
            "description": "slurm controller status"
          }
        }
      },
      "v0.0.36_partition": {
        "type": "object",
        "properties": {
          "flags": {
            "type": "array",
            "description": "partition options",
            "items": {
              "type": "string"
            }
          },
          "preemption_mode": {
            "type": "string",
            "description": "preemption type"
          },
          "allowed_allocation_nodes": {
            "type": "string",
            "description": "list names of allowed allocating nodes"
          },
          "allowed_accounts": {
            "type": "string",
            "description": "comma delimited list of accounts"
          },
          "allowed_groups": {
            "type": "string",
            "description": "comma delimited list of groups"
          },
          "allowed_qos": {
            "type": "string",
            "description": "comma delimited list of qos"
          },
          "alternative": {
            "type": "string",
            "description": "name of alternate partition"
          },
          "billing_weights": {
            "type": "string",
            "description": "TRES billing weights"
          },
          "default_memory_per_cpu": {
            "type": "integer",
            "format": "int64",
            "description": "default MB memory per allocated CPU"
          },
          "default_time_limit": {
            "type": "integer",
            "format": "int64",
            "description": "default time limit (minutes)"
          },
          "denied_accounts": {
            "type": "string",
            "description": "comma delimited list of denied accounts"
          },
          "denied_qos": {
            "type": "string",
            "description": "comma delimited list of denied qos"
          },
          "preemption_grace_time": {
            "type": "integer",
            "format": "int64",
            "description": "preemption grace time (seconds)"
          },
          "maximum_cpus_per_node": {
            "type": "integer",
            "description": "maximum allocated CPUs per node"
          },
          "maximum_memory_per_node": {
            "type": "integer",
            "format": "int64",
            "description": "maximum memory per allocated CPU (MiB)"
          },
          "maximum_nodes_per_job": {
            "type": "integer",
            "description": "Max nodes per job"
          },
          "max_time_limit": {
            "type": "integer",
            "format": "int64",
            "description": "Max time limit per job"
          },
          "min_nodes_per_job": {
            "type": "integer",
            "description": "Min number of nodes per job"
          },
          "name": {
            "type": "string",
            "description": "Partition name"
          },
          "nodes": {
            "type": "string",
            "description": "list names of nodes in partition"
          },
          "over_time_limit": {
            "type": "integer",
            "description": "job's time limit can be exceeded by this number of minutes before cancellation"
          },
          "priority_job_factor": {
            "type": "integer",
            "description": "job priority weight factor"
          },
          "priority_tier": {
            "type": "integer",
            "description": "tier for scheduling and preemption"
          },
          "qos": {
            "type": "string",
            "description": "partition QOS name"
          },
          "nodes_online": {
            "type": "integer",
            "description": "Nodes online (ready for jobs)"
          },
          "total_cpus": {
            "type": "integer",
            "description": "Total cpus in partition"
          },
          "total_nodes": {
            "type": "integer",
            "description": "Total number of nodes in partition"
          },
          "tres": {
            "type": "string",
            "description": "configured TRES in partition"
          }
        }
      },
      "v0.0.36_partitions_response": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_error"
            }
          },
          "partitions": {
            "type": "array",
            "description": "partition info",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_partition"
            }
          }
        }
      },
      "v0.0.36_error": {
        "type": "object",
        "properties": {
          "error": {
            "type": "string",
            "description": "error message"
          },
          "errno": {
            "type": "integer",
            "description": "error number"
          }
        }
      },
      "v0.0.36_signal": {
        "type": "string",
        "description": "POSIX signal name",
        "format": "int32",
        "enum": [
          "HUP",
          "INT",
          "QUIT",
          "ABRT",
          "KILL",
          "ALRM",
          "TERM",
          "USR1",
          "USR2",
          "URG",
          "CONT",
          "STOP",
          "TSTP",
          "TTIN",
          "TTOU"
        ]
      },
      "v0.0.36_job_submission_response": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_error"
            }
          },
          "job_id": {
            "description": "new job ID",
            "type": "integer"
          },
          "step_id": {
            "description": "new job step ID",
            "type": "string"
          },
          "job_submit_user_msg": {
            "description": "Message to user from job_submit plugin",
            "type": "string"
          }
        }
      },
      "v0.0.36_job_submission": {
        "required": [
          "script"
        ],
        "properties": {
          "script": {
            "type": "string",
            "description": "Executable script (full contents) to run in batch step"
          },
          "job": {
            "description": "Properties of an array job or non-HetJob",
            "$ref": "#\/components\/schemas\/v0.0.36_job_properties"
          },
          "jobs": {
            "description": "Properties of an HetJob",
            "type": "array",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_job_properties"
            }
          }
        }
      },
      "v0.0.36_jobs_response": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_error"
            }
          },
          "jobs": {
            "type": "array",
            "description": "job descriptions",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_job_response_properties"
            }
          }
        }
      },
      "v0.0.36_job_response_properties": {
        "type": "object",
        "properties": {
          "account": {
            "type": "string",
            "description": "Charge resources used by this job to specified account"
          },
          "accrue_time": {
            "type": "string",
            "description": "time job is eligible for running"
          },
          "admin_comment": {
            "type": "string",
            "description": "administrator's arbitrary comment"
          },
          "array_job_id": {
            "type": "string",
            "description": "job_id of a job array or 0 if N\/A"
          },
          "array_task_id": {
            "type": "string",
            "description": "task_id of a job array"
          },
          "array_max_tasks": {
            "type": "string",
            "description": "Maximum number of running array tasks"
          },
          "array_task_string": {
            "type": "string",
            "description": "string expression of task IDs in this record"
          },
          "association_id": {
            "type": "string",
            "description": "association id for job"
          },
          "batch_features": {
            "type": "string",
            "description": "features required for batch script's node"
          },
          "batch_flag": {
            "type": "boolean",
            "description": "if batch: queued job with script"
          },
          "batch_host": {
            "type": "string",
            "description": "name of host running batch script"
          },
          "flags": {
            "type": "array",
            "description": "Job flags",
            "items": {
              "type": "string"
            }
          },
          "burst_buffer": {
            "type": "string",
            "description": "burst buffer specifications"
          },
          "burst_buffer_state": {
            "type": "string",
            "description": "burst buffer state info"
          },
          "cluster": {
            "type": "string",
            "description": "name of cluster that the job is on"
          },
          "cluster_features": {
            "type": "string",
            "description": "comma separated list of required cluster features"
          },
          "command": {
            "type": "string",
            "description": "command to be executed"
          },
          "comment": {
            "type": "string",
            "description": "arbitrary comment"
          },
          "contiguous": {
            "type": "boolean",
            "description": "job requires contiguous nodes"
          },
          "core_spec": {
            "type": "string",
            "description": "specialized core count"
          },
          "thread_spec": {
            "type": "string",
            "description": "specialized thread count"
          },
          "cores_per_socket": {
            "type": "string",
            "description": "cores per socket required by job"
          },
          "billable_tres": {
            "type": "string",
            "description": "billable TRES"
          },
          "cpus_per_task": {
            "type": "string",
            "description": "number of processors required for each task"
          },
          "cpu_frequency_minimum": {
            "type": "string",
            "description": "Minimum cpu frequency"
          },
          "cpu_frequency_maximum": {
            "type": "string",
            "description": "Maximum cpu frequency"
          },
          "cpu_frequency_governor": {
            "type": "string",
            "description": "cpu frequency governor"
          },
          "cpus_per_tres": {
            "type": "string",
            "description": "semicolon delimited list of TRES=# values"
          },
          "deadline": {
            "type": "string",
            "description": "job start deadline "
          },
          "delay_boot": {
            "type": "string",
            "description": "command to be executed"
          },
          "dependency": {
            "type": "string",
            "description": "synchronize job execution with other jobs"
          },
          "derived_exit_code": {
            "type": "string",
            "description": "highest exit code of all job steps"
          },
          "eligible_time": {
            "type": "string",
            "description": "time job is eligible for running"
          },
          "end_time": {
            "type": "string",
            "description": "time of termination, actual or expected"
          },
          "excluded_nodes": {
            "type": "string",
            "description": "comma separated list of excluded nodes"
          },
          "exit_code": {
            "type": "integer",
            "description": "exit code for job"
          },
          "features": {
            "type": "string",
            "description": "comma separated list of required features"
          },
          "federation_origin": {
            "type": "string",
            "description": "Origin cluster's name"
          },
          "federation_siblings_active": {
            "type": "string",
            "description": "string of active sibling names"
          },
          "federation_siblings_viable": {
            "type": "string",
            "description": "string of viable sibling names"
          },
          "gres_detail": {
            "type": "array",
            "description": "Job flags",
            "items": {
              "type": "string"
            }
          },
          "group_id": {
            "type": "string",
            "description": "group job submitted as"
          },
          "job_id": {
            "type": "string",
            "description": "job ID"
          },
          "job_resources": {
            "$ref": "#\/components\/schemas\/v0.0.36_job_resources"
          },
          "job_state": {
            "type": "string",
            "description": "state of the job"
          },
          "last_sched_evaluation": {
            "type": "string",
            "description": "last time job was evaluated for scheduling"
          },
          "licenses": {
            "type": "string",
            "description": "licenses required by the job"
          },
          "max_cpus": {
            "type": "string",
            "description": "maximum number of cpus usable by job"
          },
          "max_nodes": {
            "type": "string",
            "description": "maximum number of nodes usable by job"
          },
          "mcs_label": {
            "type": "string",
            "description": "mcs_label if mcs plugin in use"
          },
          "memory_per_tres": {
            "type": "string",
            "description": "semicolon delimited list of TRES=# values"
          },
          "name": {
            "type": "string",
            "description": "name of the job"
          },
          "nodes": {
            "type": "string",
            "description": "list of nodes allocated to job"
          },
          "nice": {
            "type": "string",
            "description": "requested priority change"
          },
          "tasks_per_core": {
            "type": "string",
            "description": "number of tasks to invoke on each core"
          },
          "tasks_per_socket": {
            "type": "string",
            "description": "number of tasks to invoke on each socket"
          },
          "tasks_per_board": {
            "type": "string",
            "description": "number of tasks to invoke on each board"
          },
          "cpus": {
            "type": "string",
            "description": "minimum number of cpus required by job"
          },
          "node_count": {
            "type": "string",
            "description": "minimum number of nodes required by job"
          },
          "tasks": {
            "type": "string",
            "description": "requested task count"
          },
          "het_job_id": {
            "type": "string",
            "description": "job ID of hetjob leader"
          },
          "het_job_id_set": {
            "type": "string",
            "description": "job IDs for all components"
          },
          "het_job_offset": {
            "type": "string",
            "description": "HetJob component offset from leader"
          },
          "partition": {
            "type": "string",
            "description": "name of assigned partition"
          },
          "memory_per_node": {
            "type": "string",
            "description": "minimum real memory per node"
          },
          "memory_per_cpu": {
            "type": "string",
            "description": "minimum real memory per cpu"
          },
          "minimum_cpus_per_node": {
            "type": "string",
            "description": "minimum # CPUs per node"
          },
          "minimum_tmp_disk_per_node": {
            "type": "string",
            "description": "minimum tmp disk per node"
          },
          "preempt_time": {
            "type": "string",
            "description": "preemption signal time"
          },
          "pre_sus_time": {
            "type": "string",
            "description": "time job ran prior to last suspend"
          },
          "priority": {
            "type": "string",
            "description": "relative priority of the job"
          },
          "profile": {
            "type": "array",
            "description": "Job profiling requested",
            "items": {
              "type": "string"
            }
          },
          "qos": {
            "type": "string",
            "description": "Quality of Service"
          },
          "reboot": {
            "type": "boolean",
            "description": "node reboot requested before start"
          },
          "required_nodes": {
            "type": "string",
            "description": "comma separated list of required nodes"
          },
          "requeue": {
            "type": "boolean",
            "description": "enable or disable job requeue option"
          },
          "resize_time": {
            "type": "string",
            "description": "time of latest size change"
          },
          "restart_cnt": {
            "type": "string",
            "description": "count of job restarts"
          },
          "resv_name": {
            "type": "string",
            "description": "reservation name"
          },
          "shared": {
            "type": "string",
            "description": "type and if job can share nodes with other jobs"
          },
          "show_flags": {
            "type": "array",
            "description": "details requested",
            "items": {
              "type": "string"
            }
          },
          "sockets_per_board": {
            "type": "string",
            "description": "sockets per board required by job"
          },
          "sockets_per_node": {
            "type": "string",
            "description": "sockets per node required by job"
          },
          "start_time": {
            "type": "string",
            "description": "time execution begins, actual or expected"
          },
          "state_description": {
            "type": "string",
            "description": "optional details for state_reason"
          },
          "state_reason": {
            "type": "string",
            "description": "reason job still pending or failed"
          },
          "standard_error": {
            "type": "string",
            "description": "pathname of job's stderr file"
          },
          "standard_input": {
            "type": "string",
            "description": "pathname of job's stdin file"
          },
          "standard_output": {
            "type": "string",
            "description": "pathname of job's stdout file"
          },
          "submit_time": {
            "type": "string",
            "description": "time of job submission"
          },
          "suspend_time": {
            "type": "string",
            "description": "time job last suspended or resumed"
          },
          "system_comment": {
            "type": "string",
            "description": "slurmctld's arbitrary comment"
          },
          "time_limit": {
            "type": "string",
            "description": "maximum run time in minutes"
          },
          "time_minimum": {
            "type": "string",
            "description": "minimum run time in minutes"
          },
          "threads_per_core": {
            "type": "string",
            "description": "threads per core required by job"
          },
          "tres_bind": {
            "type": "string",
            "description": "Task to TRES binding directives"
          },
          "tres_freq": {
            "type": "string",
            "description": "TRES frequency directives"
          },
          "tres_per_job": {
            "type": "string",
            "description": "semicolon delimited list of TRES=# values"
          },
          "tres_per_node": {
            "type": "string",
            "description": "semicolon delimited list of TRES=# values"
          },
          "tres_per_socket": {
            "type": "string",
            "description": "semicolon delimited list of TRES=# values"
          },
          "tres_per_task": {
            "type": "string",
            "description": "semicolon delimited list of TRES=# values"
          },
          "tres_req_str": {
            "type": "string",
            "description": "tres reqeusted in the job"
          },
          "tres_alloc_str": {
            "type": "string",
            "description": "tres used in the job"
          },
          "user_id": {
            "type": "string",
            "description": "user id the job runs as"
          },
          "user_name": {
            "type": "string",
            "description": "user the job runs as"
          },
          "wckey": {
            "type": "string",
            "description": "wckey for job"
          },
          "current_working_directory": {
            "type": "string",
            "description": "pathname of working directory"
          }
        }
      },
      "v0.0.36_job_resources": {
        "type": "object",
        "properties": {
          "nodes": {
            "type": "string",
            "description": "list of assigned job nodes"
          },
          "allocated_cpus": {
            "type": "integer",
            "description": "number of assigned job cpus"
          },
          "allocated_hosts": {
            "type": "integer",
            "description": "number of assigned job hosts"
          },
          "allocated_nodes": {
            "type": "array",
            "description": "node allocations",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_node_allocation"
            }
          }
        }
      },
      "v0.0.36_node_allocation": {
        "type": "object",
        "properties": {
          "memory": {
            "type": "integer",
            "description": "amount of assigned job memory"
          },
          "cpus": {
            "type": "object",
            "description": "amount of assigned job CPUs"
          },
          "sockets": {
            "type": "object",
            "description": "assignment status of each socket by socket id"
          },
          "cores": {
            "type": "object",
            "description": "assignment status of each core by core id"
          }
        }
      },
      "v0.0.36_job_properties": {
        "type": "object",
        "required": [
          "environment"
        ],
        "properties": {
          "account": {
            "type": "string",
            "description": "Charge resources used by this job to specified account."
          },
          "account_gather_freqency": {
            "type": "string",
            "description": "Define the job accounting and profiling sampling intervals."
          },
          "argv": {
            "type": "array",
            "description": "Arguments to the script.",
            "items": {
              "type": "string"
            }
          },
          "array": {
            "type": "string",
            "description": "Submit a job array, multiple jobs to be executed with identical parameters. The indexes specification identifies what array index values should be used."
          },
          "batch_features": {
            "type": "string",
            "description": "features required for batch script's node"
          },
          "begin_time": {
            "type": "string",
            "description": "Submit the batch script to the Slurm controller immediately, like normal, but tell the controller to defer the allocation of the job until the specified time."
          },
          "burst_buffer": {
            "type": "string",
            "description": "Burst buffer specification."
          },
          "cluster_constraints": {
            "type": "string",
            "description": "Specifies features that a federated cluster must have to have a sibling job submitted to it."
          },
          "comment": {
            "type": "string",
            "description": "An arbitrary comment."
          },
          "constraints": {
            "type": "string",
            "description": "node features required by job."
          },
          "core_specification": {
            "type": "integer",
            "description": "Count of specialized threads per node reserved by the job for system operations and not used by the application."
          },
          "cores_per_socket": {
            "type": "integer",
            "description": "Restrict node selection to nodes with at least the specified number of cores per socket."
          },
          "cpu_binding": {
            "type": "string",
            "description": "Cpu binding"
          },
          "cpu_binding_hint": {
            "type": "string",
            "description": "Cpu binding hint"
          },
          "cpu_frequency": {
            "type": "string",
            "description": "Request that job steps initiated by srun commands inside this sbatch script be run at some requested frequency if possible, on the CPUs selected for the step on the compute node(s)."
          },
          "cpus_per_gpu": {
            "type": "string",
            "description": "Number of CPUs requested per allocated GPU."
          },
          "cpus_per_task": {
            "type": "integer",
            "description": "Advise the Slurm controller that ensuing job steps will require ncpus number of processors per task."
          },
          "current_working_directory": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard output directly to the file name."
          },
          "deadline": {
            "type": "string",
            "description": "Remove the job if no ending is possible before this deadline (start > (deadline - time[-min]))."
          },
          "delay_boot": {
            "type": "integer",
            "description": "Do not reboot nodes in order to satisfied this job's feature specification if the job has been eligible to run for less than this time period."
          },
          "dependency": {
            "type": "string",
            "description": "Defer the start of this job until the specified dependencies have been satisfied completed."
          },
          "distribution": {
            "type": "string",
            "description": "Specify alternate distribution methods for remote processes."
          },
          "environment": {
            "type": "object",
            "description": "Dictionary of environment entries."
          },
          "exclusive": {
            "type": "string",
            "description": "The job allocation can share nodes just other users with the \"user\" option or with the \"mcs\" option).",
            "enum": [
              "user",
              "mcs",
              "true",
              "false"
            ]
          },
          "get_user_environment": {
            "type": "boolean",
            "description": "Load new login environment for user on job node."
          },
          "gres": {
            "type": "string",
            "description": "Specifies a comma delimited list of generic consumable resources."
          },
          "gres_flags": {
            "type": "string",
            "description": "Specify generic resource task binding options.",
            "enum": [
              "disable-binding",
              "enforce-binding"
            ]
          },
          "gpu_binding": {
            "type": "string",
            "description": "Requested binding of tasks to GPU."
          },
          "gpu_frequency": {
            "type": "string",
            "description": "Requested GPU frequency."
          },
          "gpus": {
            "type": "string",
            "description": "GPUs per job."
          },
          "gpus_per_node": {
            "type": "string",
            "description": "GPUs per node."
          },
          "gpus_per_socket": {
            "type": "string",
            "description": "GPUs per socket."
          },
          "gpus_per_task": {
            "type": "string",
            "description": "GPUs per task."
          },
          "hold": {
            "type": "boolean",
            "description": "Specify the job is to be submitted in a held state (priority of zero)."
          },
          "kill_on_invalid_dependency": {
            "type": "boolean",
            "description": "If a job has an invalid dependency, then Slurm is to terminate it."
          },
          "licenses": {
            "type": "string",
            "description": "Specification of licenses (or other resources available on all nodes of the cluster) which must be allocated to this job."
          },
          "mail_type": {
            "type": "string",
            "description": "Notify user by email when certain event types occur."
          },
          "mail_user": {
            "type": "string",
            "description": "User to receive email notification of state changes as defined by mail_type."
          },
          "mcs_label": {
            "type": "string",
            "description": "This parameter is a group among the groups of the user."
          },
          "memory_binding": {
            "type": "string",
            "description": "Bind tasks to memory."
          },
          "memory_per_cpu": {
            "type": "integer",
            "description": "Minimum real memory per cpu (MB)."
          },
          "memory_per_gpu": {
            "type": "integer",
            "description": "Minimum memory required per allocated GPU."
          },
          "memory_per_node": {
            "type": "integer",
            "description": "Minimum real memory per node (MB)."
          },
          "minimum_cpus_per_node": {
            "type": "integer",
            "description": "Minimum number of CPUs per node."
          },
          "minimum_nodes": {
            "type": "boolean",
            "description": "If a range of node counts is given, prefer the smaller count."
          },
          "name": {
            "type": "string",
            "description": "Specify a name for the job allocation."
          },
          "nice": {
            "type": "string",
            "description": "Run the job with an adjusted scheduling priority within Slurm."
          },
          "no_kill": {
            "type": "boolean",
            "description": "Do not automatically terminate a job if one of the nodes it has been allocated fails."
          },
          "nodes": {
            "maxItems": 2,
            "minItems": 1,
            "type": "array",
            "description": "Request that a minimum of minnodes nodes and a maximum node count.",
            "items": {
              "type": "integer"
            }
          },
          "open_mode": {
            "type": "string",
            "description": "Open the output and error files using append or truncate mode as specified.",
            "default": "append",
            "enum": [
              "append",
              "truncate"
            ]
          },
          "partition": {
            "type": "string",
            "description": "Request a specific partition for the resource allocation."
          },
          "priority": {
            "type": "string",
            "description": "Request a specific job priority."
          },
          "qos": {
            "type": "string",
            "description": "Request a quality of service for the job."
          },
          "requeue": {
            "type": "boolean",
            "description": "Specifies that the batch job should eligible to being requeue."
          },
          "reservation": {
            "type": "string",
            "description": "Allocate resources for the job from the named reservation."
          },
          "signal": {
            "pattern": "[B:]<sig_num>[@<sig_time>]",
            "type": "string",
            "description": "When a job is within sig_time seconds of its end time, send it the signal sig_num."
          },
          "sockets_per_node": {
            "type": "integer",
            "description": "Restrict node selection to nodes with at least the specified number of sockets."
          },
          "spread_job": {
            "type": "boolean",
            "description": "Spread the job allocation over as many nodes as possible and attempt to evenly distribute tasks across the allocated nodes."
          },
          "standard_error": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard error directly to the file name."
          },
          "standard_in": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard input directly to the file name specified."
          },
          "standard_out": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard output directly to the file name."
          },
          "tasks": {
            "type": "integer",
            "description": "Advises the Slurm controller that job steps run within the allocation will launch a maximum of number tasks and to provide for sufficient resources."
          },
          "tasks_per_core": {
            "type": "integer",
            "description": "Request the maximum ntasks be invoked on each core."
          },
          "tasks_per_node": {
            "type": "integer",
            "description": "Request the maximum ntasks be invoked on each node."
          },
          "tasks_per_socket": {
            "type": "integer",
            "description": "Request the maximum ntasks be invoked on each socket."
          },
          "thread_specification": {
            "type": "integer",
            "description": "Count of specialized threads per node reserved by the job for system operations and not used by the application."
          },
          "threads_per_core": {
            "type": "integer",
            "description": "Restrict node selection to nodes with at least the specified number of threads per core."
          },
          "time_limit": {
            "type": "integer",
            "description": "Step time limit."
          },
          "time_minimum": {
            "type": "integer",
            "description": "Minimum run time in minutes."
          },
          "wait_all_nodes": {
            "type": "boolean",
            "description": "Do not begin execution until all nodes are ready for use."
          },
          "wckey": {
            "type": "string",
            "description": "Specify wckey to be used with job."
          }
        }
      },
      "v0.0.36_node": {
        "type": "object",
        "properties": {
          "architecture": {
            "type": "string",
            "description": "computer architecture"
          },
          "burstbuffer_network_address": {
            "type": "string",
            "description": "BcastAddr"
          },
          "boards": {
            "type": "integer",
            "description": "total number of boards per node"
          },
          "boot_time": {
            "type": "integer",
            "format": "int64",
            "description": "timestamp of node boot"
          },
          "comment": {
            "type": "string",
            "description": "Arbitrary comment"
          },
          "cores": {
            "type": "integer",
            "description": "number of cores per socket"
          },
          "cpu_binding": {
            "type": "integer",
            "description": "Default task binding"
          },
          "cpu_load": {
            "type": "integer",
            "format": "int64",
            "description": "CPU load * 100"
          },
          "free_memory": {
            "type": "integer",
            "description": "free memory in MiB"
          },
          "cpus": {
            "type": "integer",
            "description": "configured count of cpus running on the node"
          },
          "features": {
            "type": "string",
            "description": ""
          },
          "active_features": {
            "type": "string",
            "description": "list of a node's available features"
          },
          "gres": {
            "type": "string",
            "description": "list of a node's generic resources"
          },
          "gres_drained": {
            "type": "string",
            "description": "list of drained GRES"
          },
          "gres_used": {
            "type": "string",
            "description": "list of GRES in current use"
          },
          "mcs_label": {
            "type": "string",
            "description": "mcs label if mcs plugin in use"
          },
          "name": {
            "type": "string",
            "description": "node name to slurm"
          },
          "next_state_after_reboot": {
            "type": "string",
            "description": ""
          },
          "address": {
            "type": "string",
            "description": "state after reboot"
          },
          "hostname": {
            "type": "string",
            "description": "node's hostname"
          },
          "state": {
            "type": "string",
            "description": "current node state"
          },
          "operating_system": {
            "type": "string",
            "description": "operating system"
          },
          "owner": {
            "type": "string",
            "description": "User allowed to use this node"
          },
          "port": {
            "type": "integer",
            "description": "TCP port number of the slurmd"
          },
          "real_memory": {
            "type": "integer",
            "description": "configured MB of real memory on the node"
          },
          "reason": {
            "type": "string",
            "description": "reason for node being DOWN or DRAINING"
          },
          "reason_changed_at": {
            "type": "integer",
            "description": "Time stamp when reason was set"
          },
          "reason_set_by_user": {
            "type": "string",
            "description": "User that set the reason"
          },
          "slurmd_start_time": {
            "type": "integer",
            "format": "int64",
            "description": "timestamp of slurmd startup"
          },
          "sockets": {
            "type": "integer",
            "description": "total number of sockets per node"
          },
          "threads": {
            "type": "integer",
            "description": "number of threads per core"
          },
          "temporary_disk": {
            "type": "integer",
            "description": "configured MB of total disk in TMP_FS"
          },
          "weight": {
            "type": "integer",
            "description": "arbitrary priority of node for scheduling"
          },
          "tres": {
            "type": "string",
            "description": "TRES on node"
          },
          "slurmd_version": {
            "type": "string",
            "description": "Slurmd version"
          }
        }
      },
      "v0.0.36_nodes_response": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_error"
            }
          },
          "nodes": {
            "type": "array",
            "description": "nodes info",
            "items": {
              "$ref": "#\/components\/schemas\/v0.0.36_node"
            }
          }
        }
      },
      "dbv0.0.36_diag": {
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "time_start": {
            "type": "integer",
            "description": "Unix timestamp of start time"
          },
          "rollups": {
            "type": "array",
            "items": {
              "type": "object",
              "description": "Rollup statistics",
              "properties": {
                "type": {
                  "type": "string",
                  "description": "Type of rollup"
                },
                "last_run": {
                  "type": "integer",
                  "description": "Timestamp of last rollup"
                },
                "last_cycle": {
                  "type": "integer",
                  "description": "Timestamp of last cycle"
                },
                "max_cycle": {
                  "type": "integer",
                  "description": "Max time of all cycles"
                },
                "total_time": {
                  "type": "integer",
                  "description": "Total time (s) spent doing rollup"
                },
                "mean_cycles": {
                  "type": "integer",
                  "description": "Average time (s) of cycle"
                }
              }
            }
          },
          "RPCs": {
            "type": "array",
            "items": {
              "type": "object",
              "description": "Statistics by RPC type",
              "properties": {
                "rpc": {
                  "type": "string",
                  "description": "RPC type"
                },
                "count": {
                  "type": "integer",
                  "description": "Number of RPCs"
                },
                "time": {
                  "type": "object",
                  "description": "Time values",
                  "properties": {
                    "average": {
                      "type": "integer",
                      "description": "Average time spent processing this RPC type"
                    },
                    "total": {
                      "type": "integer",
                      "description": "Total time spent processing this RPC type"
                    }
                  }
                }
              }
            }
          },
          "users": {
            "type": "array",
            "items": {
              "type": "object",
              "description": "Statistics by user RPCs",
              "properties": {
                "user": {
                  "type": "string",
                  "description": "User name"
                },
                "count": {
                  "type": "integer",
                  "description": "Number of RPCs"
                },
                "time": {
                  "type": "object",
                  "description": "Time values",
                  "properties": {
                    "average": {
                      "type": "integer",
                      "description": "Average time spent processing each user RPC"
                    },
                    "total": {
                      "type": "integer",
                      "description": "Total time spent processing each user RPC"
                    }
                  }
                }
              }
            }
          }
        }
      },
      "dbv0.0.36_account": {
        "type": "object",
        "description": "Account description",
        "properties": {
          "associations": {
            "type": "array",
            "description": "List of assigned associations",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_association_short_info"
            }
          },
          "coordinators": {
            "type": "array",
            "description": "List of assigned coordinators",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_coordinator_info"
            }
          },
          "description": {
            "type": "string",
            "description": "Description of account"
          },
          "name": {
            "type": "string",
            "description": "Name of account"
          },
          "organization": {
            "type": "string",
            "description": "Assigned organization of account"
          },
          "flags": {
            "type": "array",
            "description": "List of properties of account",
            "items": {
              "type": "string",
              "description": "String of property name"
            }
          }
        }
      },
      "dbv0.0.36_account_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "accounts": {
            "type": "array",
            "description": "List of accounts",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_account"
            }
          }
        }
      },
      "dbv0.0.36_coordinator_info": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "Name of user"
          },
          "direct": {
            "type": "integer",
            "description": "If user is coordinator of this account directly or coordinator status was inheirted from a higher account in the tree"
          }
        }
      },
      "dbv0.0.36_association_short_info": {
        "type": "object",
        "properties": {
          "account": {
            "type": "string",
            "description": "Account name"
          },
          "cluster": {
            "type": "string",
            "description": "Cluster name"
          },
          "partition": {
            "type": "string",
            "description": "Partition name (optional)"
          },
          "user": {
            "type": "string",
            "description": "User name"
          }
        }
      },
      "dbv0.0.36_response_account_delete": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_response_wckey_add": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_wckey_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "wckeys": {
            "type": "array",
            "description": "List of wckeys",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_wckey"
            }
          }
        }
      },
      "dbv0.0.36_wckey": {
        "type": "object",
        "properties": {
          "accounts": {
            "type": "array",
            "description": "List of assigned accounts",
            "items": {
              "type": "string",
              "description": "Account name"
            }
          },
          "cluster": {
            "type": "string",
            "description": "Cluster name"
          },
          "id": {
            "type": "integer",
            "description": "wckey database unique id"
          },
          "name": {
            "type": "string",
            "description": "wckey name"
          },
          "user": {
            "type": "string",
            "description": "wckey user"
          },
          "flags": {
            "type": "array",
            "description": "List of properties of wckey",
            "items": {
              "type": "string",
              "description": "String of property name"
            }
          }
        }
      },
      "dbv0.0.36_response_wckey_delete": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_response_cluster_add": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_cluster_info": {
        "type": "object",
        "properties": {
          "controller": {
            "type": "object",
            "description": "Information about controller",
            "properties": {
              "host": {
                "type": "string",
                "description": "Hostname"
              },
              "port": {
                "type": "integer",
                "description": "Port number"
              }
            }
          },
          "flags": {
            "type": "array",
            "description": "List of properties of cluster",
            "items": {
              "type": "string",
              "description": "String of property name"
            }
          },
          "name": {
            "type": "string",
            "description": "Cluster name"
          },
          "nodes": {
            "type": "string",
            "description": "Assigned nodes"
          },
          "select_plugin": {
            "type": "string",
            "description": "Configured select plugin"
          },
          "associations": {
            "type": "object",
            "description": "Information about associations",
            "properties": {
              "root": {
                "$ref": "#\/components\/schemas\/dbv0.0.36_association_short_info"
              }
            }
          },
          "rpc_version": {
            "type": "integer",
            "description": "Number rpc version"
          },
          "tres": {
            "type": "array",
            "description": "List of TRES in cluster",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_response_tres"
            }
          }
        }
      },
      "dbv0.0.36_response_cluster_delete": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_response_user_update": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_user": {
        "type": "object",
        "description": "User description",
        "properties": {
          "administrator_level": {
            "type": "string",
            "description": "Description of administrator level"
          },
          "associations": {
            "type": "object",
            "description": "Assigned associations",
            "properties": {
              "root": {
                "$ref": "#\/components\/schemas\/dbv0.0.36_association_short_info"
              }
            }
          },
          "coordinators": {
            "type": "array",
            "description": "List of assigned coordinators",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_coordinator_info"
            }
          },
          "default": {
            "type": "object",
            "description": "Default settings",
            "properties": {
              "account": {
                "type": "string",
                "description": "Default account name"
              },
              "wckey": {
                "type": "string",
                "description": "Default wckey"
              }
            }
          },
          "name": {
            "type": "string",
            "description": "User name"
          }
        }
      },
      "dbv0.0.36_user_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "users": {
            "type": "array",
            "description": "Array of users",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_user"
            }
          }
        }
      },
      "dbv0.0.36_response_user_delete": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_response_association_delete": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_association": {
        "type": "object",
        "description": "Association description",
        "properties": {
          "account": {
            "type": "string",
            "description": "Assigned account"
          },
          "cluster": {
            "type": "string",
            "description": "Assigned cluster"
          },
          "default": {
            "type": "object",
            "description": "Default settings",
            "properties": {
              "qos": {
                "type": "string",
                "description": "Default QOS"
              }
            }
          },
          "flags": {
            "type": "array",
            "description": "List of properties of association",
            "items": {
              "type": "string",
              "description": "String of property name"
            }
          },
          "max": {
            "type": "object",
            "description": "Max settings",
            "properties": {
              "jobs": {
                "type": "object",
                "description": "Max jobs settings",
                "properties": {
                  "per": {
                    "type": "object",
                    "description": "Max jobs per settings",
                    "properties": {
                      "wall_clock": {
                        "type": "integer",
                        "description": "Max wallclock per job"
                      }
                    }
                  }
                }
              },
              "per": {
                "type": "object",
                "description": "Max per settings",
                "properties": {
                  "account": {
                    "type": "object",
                    "description": "Max per accounting settings",
                    "properties": {
                      "wall_clock": {
                        "type": "integer",
                        "description": "Max wallclock per account"
                      }
                    }
                  }
                }
              },
              "tres": {
                "type": "object",
                "description": "Max TRES settings",
                "properties": {
                  "per": {
                    "type": "object",
                    "description": "Max TRES per settings",
                    "properties": {
                      "job": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      },
                      "node": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      }
                    }
                  },
                  "total": {
                    "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                  },
                  "minutes": {
                    "type": "object",
                    "description": "Max TRES minutes settings",
                    "properties": {
                      "per": {
                        "type": "object",
                        "description": "Max TRES minutes per settings",
                        "properties": {
                          "job": {
                            "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                          }
                        }
                      },
                      "total": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      }
                    }
                  }
                }
              }
            }
          },
          "min": {
            "type": "object",
            "description": "Min settings",
            "properties": {
              "priority_threshold": {
                "type": "integer",
                "description": "Min priority threshold"
              }
            }
          },
          "parent_account": {
            "type": "string",
            "description": "Parent account name"
          },
          "partition": {
            "type": "string",
            "description": "Assigned partition"
          },
          "priority": {
            "type": "integer",
            "description": "Assigned priority"
          },
          "qos": {
            "type": "array",
            "description": "Assigned QOS",
            "items": {
              "type": "string",
              "description": "Assigned single QOS name"
            }
          },
          "shares_raw": {
            "type": "integer",
            "description": "Raw fairshare shares"
          },
          "usage": {
            "type": "object",
            "description": "Association usage",
            "properties": {
              "accrue_job_count": {
                "type": "integer",
                "description": "Jobs accuring priority"
              },
              "group_used_wallclock": {
                "type": "number",
                "description": "Group used wallclock time (s)"
              },
              "fairshare_factor": {
                "type": "number",
                "description": "Fairshare factor"
              },
              "fairshare_shares": {
                "type": "integer",
                "description": "Fairshare shares"
              },
              "normalized_priority": {
                "type": "integer",
                "description": "Currently active jobs"
              },
              "normalized_shares": {
                "type": "number",
                "description": "Normalized shares"
              },
              "effective_normalized_usage": {
                "type": "number",
                "description": "Effective normalized usage"
              },
              "raw_usage": {
                "type": "integer",
                "description": "Raw usage"
              },
              "job_count": {
                "type": "integer",
                "description": "Total jobs submitted"
              },
              "fairshare_level": {
                "type": "number",
                "description": "Fairshare level"
              }
            }
          },
          "user": {
            "type": "string",
            "description": "Assigned user"
          }
        }
      },
      "dbv0.0.36_associations_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "associations": {
            "type": "array",
            "description": "Array of associations",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_association"
            }
          }
        }
      },
      "dbv0.0.36_qos": {
        "type": "object",
        "description": "QOS description",
        "properties": {
          "description": {
            "type": "string",
            "description": "QOS description"
          },
          "flags": {
            "type": "array",
            "description": "List of properties of QOS",
            "items": {
              "type": "string",
              "description": "String of property name"
            }
          },
          "id": {
            "type": "string",
            "description": "Database id"
          },
          "limits": {
            "type": "object",
            "description": "Assigned limits",
            "properties": {
              "max": {
                "type": "object",
                "description": "Limits on max settings",
                "properties": {
                  "wall_clock": {
                    "type": "object",
                    "description": "Limit on wallclock settings",
                    "properties": {
                      "per": {
                        "type": "object",
                        "description": "Limit on wallclock per settings",
                        "properties": {
                          "qos": {
                            "type": "integer",
                            "description": "Max wallclock per QOS"
                          },
                          "job": {
                            "type": "integer",
                            "description": "Max wallclock per job"
                          }
                        }
                      }
                    }
                  },
                  "jobs": {
                    "type": "object",
                    "description": "Limits on jobs settings",
                    "properties": {
                      "per": {
                        "type": "object",
                        "description": "Limits on jobs per settings",
                        "properties": {
                          "account": {
                            "type": "integer",
                            "description": "Max jobs per account"
                          },
                          "user": {
                            "type": "integer",
                            "description": "Max jobs per user"
                          }
                        }
                      }
                    }
                  },
                  "accruing": {
                    "type": "object",
                    "description": "Limits on accruing priority",
                    "properties": {
                      "per": {
                        "type": "object",
                        "description": "Max accuring priority per setting",
                        "properties": {
                          "account": {
                            "type": "integer",
                            "description": "Max accuring priority per account"
                          },
                          "user": {
                            "type": "integer",
                            "description": "Max accuring priority per user"
                          }
                        }
                      }
                    }
                  },
                  "tres": {
                    "type": "object",
                    "description": "Limits on TRES",
                    "properties": {
                      "minutes": {
                        "type": "object",
                        "description": "Max TRES minutes settings",
                        "properties": {
                          "per": {
                            "type": "object",
                            "description": "Max TRES minutes per settings",
                            "properties": {
                              "job": {
                                "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                              },
                              "account": {
                                "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                              },
                              "user": {
                                "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                              }
                            }
                          }
                        }
                      },
                      "per": {
                        "type": "object",
                        "description": "Max TRES per settings",
                        "properties": {
                          "account": {
                            "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                          },
                          "job": {
                            "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                          },
                          "node": {
                            "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                          },
                          "user": {
                            "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                          }
                        }
                      }
                    }
                  }
                }
              },
              "min": {
                "type": "object",
                "description": "Min limit settings",
                "properties": {
                  "priority_threshold": {
                    "type": "integer",
                    "description": "Min priority threshold"
                  },
                  "tres": {
                    "type": "object",
                    "description": "Min tres settings",
                    "properties": {
                      "per": {
                        "type": "object",
                        "description": "Min tres per settings",
                        "properties": {
                          "job": {
                            "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          },
          "preempt": {
            "type": "object",
            "description": "Preemption settings",
            "properties": {
              "list": {
                "type": "array",
                "description": "List of preemptable QOS",
                "items": {
                  "type": "string",
                  "description": "Preemptable QOS"
                }
              },
              "mode": {
                "type": "array",
                "description": "List of preemption modes",
                "items": {
                  "type": "string",
                  "description": "Preemption mode"
                }
              },
              "exempt_time": {
                "type": "integer",
                "description": "Grace period (s) before jobs can preempted"
              }
            }
          },
          "priority": {
            "type": "integer",
            "description": "QOS priority"
          },
          "usage_factor": {
            "type": "number",
            "description": "Usage factor"
          },
          "usage_threshold": {
            "type": "number",
            "description": "Usage threshold"
          }
        }
      },
      "dbv0.0.36_qos_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "qos": {
            "type": "array",
            "description": "Array of QOS",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_qos"
            }
          }
        }
      },
      "dbv0.0.36_response_qos_delete": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_response_tres": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_tres_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "tres": {
            "type": "array",
            "description": "Array of tres",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
            }
          }
        }
      },
      "dbv0.0.36_tres_list": {
        "type": "array",
        "description": "TRES list of attributes",
        "items": {
          "type": "object",
          "properties": {
            "type": {
              "type": "string",
              "description": "TRES type"
            },
            "name": {
              "type": "string",
              "description": "TRES name (optional)"
            },
            "id": {
              "type": "integer",
              "description": "database id"
            },
            "count": {
              "type": "integer",
              "description": "count of TRES"
            }
          }
        }
      },
      "dbv0.0.36_job_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "jobs": {
            "type": "array",
            "description": "Array of jobs",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_job"
            }
          }
        }
      },
      "dbv0.0.36_job": {
        "type": "object",
        "description": "Single job description",
        "properties": {
          "account": {
            "type": "string",
            "description": "Account charged by job"
          },
          "comment": {
            "type": "object",
            "description": "Job comments by type",
            "properties": {
              "administrator": {
                "type": "string",
                "description": "Administrator set comment"
              },
              "job": {
                "type": "string",
                "description": "Job comment"
              },
              "system": {
                "type": "string",
                "description": "System set comment"
              }
            }
          },
          "allocation_nodes": {
            "type": "string",
            "description": "Nodes allocated to job"
          },
          "array": {
            "type": "object",
            "description": "Array properties (optional)",
            "properties": {
              "job_id": {
                "type": "integer",
                "description": "Job id of array"
              },
              "limits": {
                "type": "object",
                "description": "Limits on array settings",
                "properties": {
                  "max": {
                    "type": "object",
                    "description": "Limits on array settings",
                    "properties": {
                      "running": {
                        "type": "object",
                        "description": "Limits on array settings",
                        "properties": {
                          "tasks": {
                            "type": "integer",
                            "description": "Max running tasks in array at any one time"
                          }
                        }
                      }
                    }
                  }
                }
              },
              "task": {
                "type": "string",
                "description": "Array task"
              },
              "task_id": {
                "type": "integer",
                "description": "Array task id"
              }
            }
          },
          "time": {
            "type": "object",
            "description": "Time properties",
            "properties": {
              "elapsed": {
                "type": "integer",
                "description": "Total time elapsed"
              },
              "eligible": {
                "type": "integer",
                "description": "Total time eligible to run"
              },
              "end": {
                "type": "integer",
                "description": "Timestamp of when job ended"
              },
              "start": {
                "type": "integer",
                "description": "Timestamp of when job started"
              },
              "submission": {
                "type": "integer",
                "description": "Timestamp of when job submitted"
              },
              "suspended": {
                "type": "integer",
                "description": "Timestamp of when job last suspended"
              },
              "system": {
                "type": "object",
                "description": "System time values",
                "properties": {
                  "seconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the system on behalf of the process (in kernel mode), in seconds"
                  },
                  "microseconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the system on behalf of the process (in kernel mode), in microseconds"
                  }
                }
              },
              "total": {
                "type": "object",
                "description": "System time values",
                "properties": {
                  "seconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job, in seconds"
                  },
                  "microseconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job, in microseconds"
                  }
                }
              },
              "user": {
                "type": "object",
                "description": "User land time values",
                "properties": {
                  "seconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job in user land, in seconds"
                  },
                  "microseconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job in user land, in microseconds"
                  }
                }
              },
              "limit": {
                "type": "integer",
                "description": "Job wall clock time limit"
              }
            }
          },
          "association": {
            "$ref": "#\/components\/schemas\/dbv0.0.36_association_short_info"
          },
          "cluster": {
            "type": "string",
            "description": "Assigned cluster"
          },
          "constraints": {
            "type": "string",
            "description": "Constraints on job"
          },
          "derived_exit_code": {
            "$ref": "#\/components\/schemas\/dbv0.0.36_job_exit_code"
          },
          "exit_code": {
            "$ref": "#\/components\/schemas\/dbv0.0.36_job_exit_code"
          },
          "flags": {
            "type": "array",
            "description": "List of properties of job",
            "items": {
              "type": "string",
              "description": "String of property name"
            }
          },
          "group": {
            "type": "string",
            "description": "User's group to run job"
          },
          "het": {
            "type": "object",
            "description": "Heterogeneous Job details (optional)",
            "properties": {
              "job_id": {
                "type": "object",
                "description": "Parent HetJob id"
              },
              "job_offset": {
                "type": "object",
                "description": "Offset of this job to parent"
              }
            }
          },
          "job_id": {
            "type": "integer",
            "description": "Job id"
          },
          "name": {
            "type": "string",
            "description": "Assigned job name"
          },
          "mcs": {
            "type": "object",
            "description": "Multi-Category Security",
            "properties": {
              "label": {
                "type": "string",
                "description": "Assigned MCS label"
              }
            }
          },
          "nodes": {
            "type": "string",
            "description": "List of nodes allocated for job"
          },
          "partition": {
            "type": "string",
            "description": "Assigned job's partition"
          },
          "priority": {
            "type": "integer",
            "description": "Priority"
          },
          "qos": {
            "type": "string",
            "description": "Assigned qos name"
          },
          "required": {
            "type": "object",
            "description": "Job run requirements",
            "properties": {
              "CPUs": {
                "type": "integer",
                "description": "Required number of CPUs"
              },
              "memory": {
                "type": "integer",
                "description": "Required amount of memory (MiB)"
              }
            }
          },
          "kill_request_user": {
            "type": "string",
            "description": "User who requested job killed"
          },
          "reservation": {
            "type": "object",
            "description": "Reservation usage details",
            "properties": {
              "id": {
                "type": "integer",
                "description": "Database id of reservation"
              },
              "name": {
                "type": "integer",
                "description": "Name of reservation"
              }
            }
          },
          "state": {
            "type": "object",
            "description": "State properties of job",
            "properties": {
              "current": {
                "type": "string",
                "description": "Current state of job"
              },
              "previous": {
                "type": "string",
                "description": "Previous state of job"
              }
            }
          },
          "steps": {
            "type": "array",
            "description": "Job step description",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_job_step"
            }
          },
          "tres": {
            "type": "object",
            "description": "TRES settings",
            "properties": {
              "allocated": {
                "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
              },
              "requested": {
                "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
              }
            }
          },
          "user": {
            "type": "string",
            "description": "Job user"
          },
          "wckey": {
            "type": "object",
            "description": "Job assigned wckey details",
            "properties": {
              "wckey": {
                "type": "string",
                "description": "Job assigned wckey"
              },
              "flags": {
                "type": "array",
                "description": "wckey flags",
                "items": {
                  "type": "string",
                  "description": "Flag string"
                }
              }
            }
          },
          "working_directory": {
            "type": "string",
            "description": "Directory where job was initially started"
          }
        }
      },
      "dbv0.0.36_job_exit_code": {
        "type": "object",
        "properties": {
          "status": {
            "type": "string",
            "description": "Job exit status"
          },
          "return_code": {
            "type": "integer",
            "description": "Return code from parent process"
          },
          "signal": {
            "type": "object",
            "description": "Signal details (if signaled)",
            "properties": {
              "signal_id": {
                "type": "integer",
                "description": "Signal number process received"
              },
              "name": {
                "type": "string",
                "description": "Name of signal received"
              }
            }
          }
        }
      },
      "dbv0.0.36_job_step": {
        "type": "object",
        "properties": {
          "time": {
            "type": "object",
            "description": "Time properties",
            "properties": {
              "elapsed": {
                "type": "integer",
                "description": "Total time elapsed"
              },
              "end": {
                "type": "integer",
                "description": "Timestamp of when job ended"
              },
              "start": {
                "type": "integer",
                "description": "Timestamp of when job started"
              },
              "suspended": {
                "type": "integer",
                "description": "Timestamp of when job last suspended"
              },
              "system": {
                "type": "object",
                "description": "System time values",
                "properties": {
                  "seconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the system on behalf of the process (in kernel mode), in seconds"
                  },
                  "microseconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the system on behalf of the process (in kernel mode), in microseconds"
                  }
                }
              },
              "total": {
                "type": "object",
                "description": "System time values",
                "properties": {
                  "seconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job, in seconds"
                  },
                  "microseconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job, in microseconds"
                  }
                }
              },
              "user": {
                "type": "object",
                "description": "User land time values",
                "properties": {
                  "seconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job in user land, in seconds"
                  },
                  "microseconds": {
                    "type": "integer",
                    "description": "Total number of CPU-seconds used by the job in user land, in microseconds"
                  }
                }
              }
            }
          },
          "exit_code": {
            "$ref": "#\/components\/schemas\/dbv0.0.36_job_exit_code"
          },
          "nodes": {
            "type": "object",
            "description": "Node details",
            "properties": {
              "count": {
                "type": "integer",
                "description": "Total number of nodes in step"
              },
              "range": {
                "type": "string",
                "description": "Nodes in step"
              }
            }
          },
          "tasks": {
            "type": "object",
            "description": "Task properties",
            "properties": {
              "count": {
                "type": "integer",
                "description": "Number of tasks in step"
              }
            }
          },
          "pid": {
            "type": "string",
            "description": "First process PID"
          },
          "CPU": {
            "type": "object",
            "description": "CPU properties",
            "properties": {
              "requested_frequency": {
                "type": "object",
                "description": "CPU frequency requested",
                "properties": {
                  "min": {
                    "type": "integer",
                    "description": "Min CPU frequency"
                  },
                  "max": {
                    "type": "integer",
                    "description": "Max CPU frequency"
                  }
                }
              },
              "governor": {
                "type": "array",
                "description": "CPU governor",
                "items": {
                  "type": "string",
                  "description": "CPU governor type"
                }
              }
            }
          },
          "kill_request_user": {
            "type": "string",
            "description": "User who requested job killed"
          },
          "state": {
            "type": "string",
            "description": "State of job step"
          },
          "statistics": {
            "type": "object",
            "description": "Statistics of job step",
            "properties": {
              "CPU": {
                "type": "object",
                "description": "Statistics of CPU",
                "properties": {
                  "actual_frequency": {
                    "type": "integer",
                    "description": "Actual frequency of CPU during step"
                  }
                }
              },
              "energy": {
                "type": "object",
                "description": "Statistics of energy",
                "properties": {
                  "consumed": {
                    "type": "integer",
                    "description": "Energy consumed during step"
                  }
                }
              }
            }
          },
          "step": {
            "type": "object",
            "description": "Step details",
            "properties": {
              "job_id": {
                "type": "integer",
                "description": "Parent job id"
              },
              "het": {
                "type": "object",
                "description": "Heterogeneous job details",
                "properties": {
                  "component": {
                    "type": "integer",
                    "description": "Parent HetJob component id"
                  }
                }
              },
              "id": {
                "type": "string",
                "description": "Step id"
              },
              "name": {
                "type": "string",
                "description": "Step name"
              },
              "task": {
                "type": "object",
                "description": "Task properties",
                "properties": {
                  "distribution": {
                    "type": "string",
                    "description": "Task distribution type"
                  }
                }
              },
              "tres": {
                "type": "object",
                "description": "TRES usage",
                "properties": {
                  "requested": {
                    "type": "object",
                    "description": "TRES requested for job",
                    "properties": {
                      "average": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      },
                      "max": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      },
                      "min": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      },
                      "total": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      }
                    }
                  },
                  "consumed": {
                    "type": "object",
                    "description": "TRES requested for job",
                    "properties": {
                      "average": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      },
                      "max": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      },
                      "min": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      },
                      "total": {
                        "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                      }
                    }
                  },
                  "allocated": {
                    "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
                  }
                }
              }
            }
          }
        }
      },
      "dbv0.0.36_config_info": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          },
          "tres": {
            "type": "array",
            "description": "Array of TRES",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_tres_list"
            }
          },
          "accounts": {
            "type": "array",
            "description": "Array of accounts",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_account"
            }
          },
          "associations": {
            "type": "array",
            "description": "Array of associations",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_association"
            }
          },
          "users": {
            "type": "array",
            "description": "Array of users",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_user"
            }
          },
          "qos": {
            "type": "array",
            "description": "Array of qos",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_qos"
            }
          },
          "wckeys": {
            "type": "array",
            "description": "Array of wckeys",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_wckey"
            }
          }
        }
      },
      "dbv0.0.36_account_response": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_config_response": {
        "type": "object",
        "properties": {
          "errors": {
            "type": "array",
            "description": "Slurm errors",
            "items": {
              "$ref": "#\/components\/schemas\/dbv0.0.36_error"
            }
          }
        }
      },
      "dbv0.0.36_error": {
        "properties": {
          "errno": {
            "description": "Error number",
            "type": "integer"
          },
          "error": {
            "description": "Error message",
            "type": "string"
          }
        },
        "type": "object"
      },
      "signal": {
        "oneOf": [
          {
            "type": "integer",
            "description": "POSIX signal number",
            "format": "int32"
          },
          {
            "type": "string",
            "description": "POSIX signal name",
            "format": "int32",
            "enum": [
              "HUP",
              "INT",
              "QUIT",
              "ABRT",
              "KILL",
              "ALRM",
              "TERM",
              "USR1",
              "USR2",
              "URG",
              "CONT",
              "STOP",
              "TSTP",
              "TTIN",
              "TTOU"
            ]
          }
        ]
      },
      "job_properties": {
        "properties": {
          "account": {
            "type": "string",
            "description": "Charge resources used by this job to specified account."
          },
          "account_gather_freqency": {
            "type": "string",
            "description": "Define the job accounting and profiling sampling intervals."
          },
          "argv": {
            "type": "array",
            "description": "Arguments to the script.",
            "items": {
              "type": "string"
            }
          },
          "array": {
            "type": "string",
            "description": "Submit a job array, multiple jobs to be executed with identical parameters. The indexes specification identifies what array index values should be used."
          },
          "batch_features": {
            "type": "string",
            "description": "features required for batch script's node"
          },
          "begin_time": {
            "type": "string",
            "description": "Submit the batch script to the Slurm controller immediately, like normal, but tell the controller to defer the allocation of the job until the specified time."
          },
          "burst_buffer": {
            "type": "string",
            "description": "Burst buffer specification."
          },
          "cluster_constraints": {
            "type": "string",
            "description": "Specifies features that a federated cluster must have to have a sibling job submitted to it."
          },
          "comment": {
            "type": "string",
            "description": "An arbitrary comment."
          },
          "constraints": {
            "type": "string",
            "description": "node features required by job."
          },
          "core_specification": {
            "type": "integer",
            "description": "Count of specialized threads per node reserved by the job for system operations and not used by the application."
          },
          "cores_per_socket": {
            "type": "integer",
            "description": "Restrict node selection to nodes with at least the specified number of cores per socket."
          },
          "cpu_binding": {
            "type": "string",
            "description": "Cpu binding"
          },
          "cpu_binding_hint": {
            "type": "string",
            "description": "Cpu binding hint"
          },
          "cpu_frequency": {
            "type": "string",
            "description": "Request that job steps initiated by srun commands inside this sbatch script be run at some requested frequency if possible, on the CPUs selected for the step on the compute node(s)."
          },
          "cpus_per_gpu": {
            "type": "string",
            "description": "Number of CPUs requested per allocated GPU."
          },
          "cpus_per_task": {
            "type": "integer",
            "description": "Advise the Slurm controller that ensuing job steps will require ncpus number of processors per task."
          },
          "current_working_directory": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard output directly to the file name."
          },
          "deadline": {
            "type": "string",
            "description": "Remove the job if no ending is possible before this deadline (start > (deadline - time[-min]))."
          },
          "delay_boot": {
            "type": "integer",
            "description": "Do not reboot nodes in order to satisfied this job's feature specification if the job has been eligible to run for less than this time period."
          },
          "dependency": {
            "type": "string",
            "description": "Defer the start of this job until the specified dependencies have been satisfied completed."
          },
          "distribution": {
            "type": "string",
            "description": "Specify alternate distribution methods for remote processes."
          },
          "environment": {
            "type": "object",
            "description": "Dictionary of environment entries."
          },
          "exclusive": {
            "oneOf": [
              {
                "type": "string",
                "description": "The job allocation can share nodes just other users with the \"user\" option or with the \"mcs\" option).",
                "enum": [
                  "user",
                  "mcs"
                ]
              },
              {
                "type": "boolean",
                "description": "Request exclusive use of nodes.",
                "default": true
              }
            ]
          },
          "get_user_environment": {
            "type": "boolean",
            "description": "Load new login environment for user on job node."
          },
          "gres": {
            "type": "string",
            "description": "Specifies a comma delimited list of generic consumable resources."
          },
          "gres_flags": {
            "type": "string",
            "description": "Specify generic resource task binding options.",
            "enum": [
              "disable-binding",
              "enforce-binding"
            ]
          },
          "gpu_binding": {
            "type": "string",
            "description": "Requested binding of tasks to GPU."
          },
          "gpu_frequency": {
            "type": "string",
            "description": "Requested GPU frequency."
          },
          "gpus": {
            "type": "string",
            "description": "GPUs per job."
          },
          "gpus_per_node": {
            "type": "string",
            "description": "GPUs per node."
          },
          "gpus_per_socket": {
            "type": "string",
            "description": "GPUs per socket."
          },
          "gpus_per_task": {
            "type": "string",
            "description": "GPUs per task."
          },
          "hold": {
            "type": "boolean",
            "description": "Specify the job is to be submitted in a held state (priority of zero)."
          },
          "kill_on_invalid_dependency": {
            "type": "boolean",
            "description": "If a job has an invalid dependency, then Slurm is to terminate it."
          },
          "licenses": {
            "type": "string",
            "description": "Specification of licenses (or other resources available on all nodes of the cluster) which must be allocated to this job."
          },
          "mail_type": {
            "type": "string",
            "description": "Notify user by email when certain event types occur."
          },
          "mail_user": {
            "type": "string",
            "description": "User to receive email notification of state changes as defined by mail_type."
          },
          "mcs_label": {
            "type": "string",
            "description": "This parameter is a group among the groups of the user."
          },
          "memory_binding": {
            "type": "string",
            "description": "Bind tasks to memory."
          },
          "memory_per_cpu": {
            "type": "integer",
            "description": "Minimum real memory per cpu (MB)."
          },
          "memory_per_gpu": {
            "type": "integer",
            "description": "Minimum memory required per allocated GPU."
          },
          "memory_per_node": {
            "type": "integer",
            "description": "Minimum real memory per node (MB)."
          },
          "minimum_cpus_per_node": {
            "type": "integer",
            "description": "Minimum number of CPUs per node."
          },
          "minimum_nodes": {
            "type": "boolean",
            "description": "If a range of node counts is given, prefer the smaller count."
          },
          "name": {
            "type": "string",
            "description": "Specify a name for the job allocation."
          },
          "nice": {
            "type": "string",
            "description": "Run the job with an adjusted scheduling priority within Slurm."
          },
          "no_kill": {
            "type": "boolean",
            "description": "Do not automatically terminate a job if one of the nodes it has been allocated fails."
          },
          "nodes": {
            "oneOf": [
              {
                "type": "integer",
                "description": "Request that a minimum of minnodes nodes be allocated to this job."
              },
              {
                "maxItems": 2,
                "minItems": 2,
                "type": "array",
                "description": "Request that a minimum of minnodes nodes and a maximum node count.",
                "items": {
                  "type": "integer"
                }
              }
            ]
          },
          "open_mode": {
            "type": "string",
            "description": "Open the output and error files using append or truncate mode as specified.",
            "default": "append",
            "enum": [
              "append",
              "truncate"
            ]
          },
          "partition": {
            "type": "string",
            "description": "Request a specific partition for the resource allocation."
          },
          "priority": {
            "type": "string",
            "description": "Request a specific job priority."
          },
          "qos": {
            "type": "string",
            "description": "Request a quality of service for the job."
          },
          "requeue": {
            "type": "boolean",
            "description": "Specifies that the batch job should eligible to being requeue."
          },
          "reservation": {
            "type": "string",
            "description": "Allocate resources for the job from the named reservation."
          },
          "signal": {
            "pattern": "[B:]<sig_num>[@<sig_time>]",
            "type": "string",
            "description": "When a job is within sig_time seconds of its end time, send it the signal sig_num."
          },
          "sockets_per_node": {
            "type": "integer",
            "description": "Restrict node selection to nodes with at least the specified number of sockets."
          },
          "spread_job": {
            "type": "boolean",
            "description": "Spread the job allocation over as many nodes as possible and attempt to evenly distribute tasks across the allocated nodes."
          },
          "standard_error": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard error directly to the file name."
          },
          "standard_in": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard input directly to the file name specified."
          },
          "standard_out": {
            "type": "string",
            "description": "Instruct Slurm to connect the batch script's standard output directly to the file name."
          },
          "tasks": {
            "type": "integer",
            "description": "Advises the Slurm controller that job steps run within the allocation will launch a maximum of number tasks and to provide for sufficient resources."
          },
          "tasks_per_core": {
            "type": "integer",
            "description": "Request the maximum ntasks be invoked on each core."
          },
          "tasks_per_node": {
            "type": "integer",
            "description": "Request the maximum ntasks be invoked on each node."
          },
          "tasks_per_socket": {
            "type": "integer",
            "description": "Request the maximum ntasks be invoked on each socket."
          },
          "thread_specification": {
            "type": "integer",
            "description": "Count of specialized threads per node reserved by the job for system operations and not used by the application."
          },
          "threads_per_core": {
            "type": "integer",
            "description": "Restrict node selection to nodes with at least the specified number of threads per core."
          },
          "time_limit": {
            "type": "integer",
            "description": "Step time limit."
          },
          "time_minimum": {
            "type": "integer",
            "description": "Minimum run time in minutes."
          },
          "wait_all_nodes": {
            "type": "boolean",
            "description": "Do not begin execution until all nodes are ready for use."
          },
          "wckey": {
            "type": "string",
            "description": "Specify wckey to be used with job."
          }
        }
      }
    },
    "securitySchemes": {
      "user": {
        "type": "apiKey",
        "description": "User name",
        "name": "X-SLURM-USER-NAME",
        "in": "header"
      },
      "token": {
        "type": "apiKey",
        "description": "User access token",
        "name": "X-SLURM-USER-TOKEN",
        "in": "header"
      }
    }
  },
  "openapi": "3.0.2",
  "info": {
    "title": "Slurm Rest API",
    "description": "API to access and control Slurm.",
    "termsOfService": "https:\/\/github.com\/SchedMD\/slurm\/blob\/master\/DISCLAIMER",
    "contact": {
      "name": "SchedMD LLC",
      "url": "https:\/\/www.schedmd.com\/",
      "email": "sales@schedmd.com"
    },
    "license": {
      "name": "Apache 2.0",
      "url": "https:\/\/www.apache.org\/licenses\/LICENSE-2.0.html"
    },
    "version": "0.0.36"
  },
  "security": [
    {
      "user": [
      ],
      "token": [
      ]
    }
  ],
  "servers": [
    {
      "url": "\/"
    }
  ]
}
```
