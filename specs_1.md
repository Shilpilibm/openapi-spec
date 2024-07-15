## openapi spec for elastic search:

    {
         "openapi": "3.0.1",
         "info": {
           "title": "Custom Search Elastic",
           "description": "Elastic Search",
           "version": "1.0"
         },

         "paths": {
           "/{index_name}/_search": {
             "post": {
               "parameters": [
                 {
                   "name": "index_name",
                   "in": "path",
                   "description": "Name of the index",
                   "required": true,
                   "schema": {
                     "type": "string"
                   }
                 }
               ],
               "security": [
                 {
                   "basicAuth": []
                 }
               ], \
               "requestBody": {
                 "content": {
                   "application/json": {
                     "schema": {
                       "type": "object",
                       "properties": {
                         "_source": {
                           "type": "array",
                           "items": {
                             "type": "string"
                           }
                         }, \
                         "knn": {
                           "type": "object"
                         }
                       }
                     },
                     "example": {
                       "query": {
                         "text_expansion": {
                           "ml.tokens": {
                             "model_id": ".elser_model_1",
                             "model_text": "tell me about a custom extension"
                           }
                         }
                       },
                       "knn": {
                         "field": "text_embedding.predicted_value",
                         "query_vector_builder": {
                           "text_embedding": {
                             "model_id": "intfloat__multilingual-e5-small",
                             "model_text": "how to set up custom extension?"
                           }
                         },
                         "k": 10,
                         "num_candidates": 100
                       }
                     }
                   }
                 }
               },
               "responses": {
                 "200": {
                   "description": "Search results returned by Elastic",
                   "content": {
                     "application/json; charset=utf-8": {
                       "schema": {
                         "type": "object",
                         "properties": {
                           "took": {
                             "type": "integer"
                           },
                           "timed_out": {
                             "type": "boolean"
                           },
                           "_shards": {
                             "type": "object",
                             "properties": {
                               "total": {
                                 "type": "integer"
                               },
                               "successful": {
                                 "type": "integer"
                               },
                               "skipped": {
                                 "type": "integer"
                               },
                               "failed": {
                                 "type": "integer"
                               }
                             }
                           },
                           "hits": {
                             "type": "object",
                             "properties": {
                               "max_score": {
                                 "type": "integer"
                               },
                               "hits": {
                                 "type": "array",
                                 "items": {
                                   "type": "object",
                                   "properties": {
                                     "_id": {
                                       "type": "string"
                                     },
                                     "_score": {
                                       "type": "number"
                                     },
                                     "_index": {
                                       "type": "string"
                                     },
                                     "_source": {
                                       "type": "object",
                                       "properties": {
                                         "section_title": {
                                           "type": "string"
                                         },
                                         "text": {
                                           "type": "string"
                                         },
                                         "title": {
                                           "type": "string"
                                         }
                                       }
                                     }
                                   }
                                 }
                               }
                             }
                           }
                         }
                       }
                     }
                   }
                 }
               }
             }
           }
         },
         "components": {
           "securitySchemes": {
             "basicAuth": {
               "type": "http",
               "scheme": "basic"
             }
           }
         }
       }

##openapi spec for watsonx.ai:

    {
        "openapi": "3.0.3",
        "info": {
          "description": "Minimal spec for commonly used features in watsonx.ai /generation API endpoint.",
          "title": "Simplified watsonx.ai generation API",
          "version": "1.1.0"
        },
        "servers": [
          {
            "url": "https://{region}.ml.cloud.ibm.com",
            "description": "watsonx.ai v1",
            "variables": {
              "region": {
                "enum": [
                  "us-south",
                  "eu-de",
                  "eu-gb",
                  "jp-tok"
                ],
                "default": "us-south",
                "description": "The region where you want to access watsonx.ai"
              }
            }
          }
        ],
        "components": {
          "securitySchemes": {
            "oauth2": {
              "type": "oauth2",
              "flows": {
              }
            }
          }
        },
        "security": [
          {
            "oauth2": []
          }
        ],
        "paths": {
          "/ml/v1/text/generation": {
            "post": {
              "description": "Generation",
              "parameters": [
                {
                  "name": "version",
                  "in": "query",
                  "description": "Release date. Specify dates in YYYY-MM-DD format.",
                  "required": true,
                  "schema": {
                    "type": "string"
                  }
                }
              ],
              "requestBody": {
                "content": {
                  "application/json": {
                    "schema": {
                      "type": "object",
                      "required": ["model_id", "input", "project_id"],
                      "properties": {
                        "model_id": {
                          "type": "string",
                          "description": "The ID of the model to be used for this request.",
                          "example": "google/flan-ul2"
                        },
                        "parameters": {
                          "type": "object",
                          "properties": {
                            "decoding_method": {
                              "type": "string",
                              "description": "The strategy used for picking the tokens during generation",
                              "example": "greedy"
                            },
                            "repetition_penalty": {
                              "type": "number",
                              "description": "The value which represents the penalty for penalizing tokens",
                              "example": "1.10"
                            },
                          }
                        }
                      }
                    }
                  }
                }
              },
              "responses": {
                "200": {
                  "description": "Default Response",
                  "content": {
                    "application/json": {
                      "schema": {
                        "type": "object",
                        "properties": {
                          "model_id": {
                            "description": "The ID of the model to be used for this request",
                            "type": "string"
                          },
                          "created_at": {
                            "description": "The date and time of the response",
                            "type": "string"
                          },
                          "results": {
                            "type": "array",
                            "items": {
                              "type": "object",
                              "properties": {
                                "generated_text": {
                                  "description": "The generated text",
                                  "type": "string"
                                },
                                "generated_token_count": {
                                  "description": "The number of tokens in the output",
                                  "type": "integer"
                                },
                                "input_token_count": {
                                  "description": "The number of tokens in the input",
                                  "type": "integer"
                                },
                                "stop_reason": {
                                  "description": "The reason for stopping the generation.",
                                  "type": "string"
                                }
                              }
                            },
                            "description": "Outputs of the generation"
                          }
                        }
                      }
                    }
                  }
                },
                "default": {
                  "description": "Unexpected error"
                }
              }
            }
          }
        }
      }

##openapi spec for milvus:

    answer: |
      {
        "openapi": "3.0.3",
        "info": {
          "title": "Milvus search",
          "description": "Unofficial simplified OpenAPI specification for the Milvus",
          "version": "1.0.0"
        },
        "servers": [
          {
            "url": "http://{milvus_url}",
            "description": "IP address and port without encryption/authentication",
            "variables": {
              "milvus_url": {
                "default": "0.0.0.0:9091",
                "description": "The portions of URL that follow http://"
              }
            }
          },
          {
            "url": "https://{milvus_url}",
            "description": "IP address and port with encryption/authentication",
            "variables": {
              "milvus_url": {
                "default": "0.0.0.0:9091",
                "description": "The portions of URL that follow https://"
              }
            }
          }
        ],
        "paths": {
          "/api/v1/search": {
            "post": {
              "summary": "Semantic (vector) search in the given collection",
              "requestBody": {
                "required": true,
                "content": {
                  "application/json": {
                    "schema": {
                      "$ref": "#/components/schemas/SearchRequest"
                    }
                  }
                }
              },
              "responses": {
                "200": {
                  "description": "Successful search",
                  "content": {
                    "application/json": {
                      "schema": {
                        "$ref": "#/components/schemas/SearchResponse"
                      }
                    }
                  }
                }
              }
            }
          }
        },
        "components": {
          "securitySchemes": {
            "basicAuth": {
              "type": "http",
              "scheme": "basic"
            }
          },
          "schemas": {
            "SearchRequest": {
              "type": "object",
              "description": "A search request made by the client",
              "properties": {
                "collection_name": {
                  "type": "string",
                  "description": "The name of the collection to search in"
                },
                "search_params": {
                  "type": "array",
                  "items": {
                    "$ref": "#/components/schemas/Param"
                  },
                  "description": "Parameters for the index"
                },
                },
                "vectors": {
                  "type": "array",
                  "items": {
                    "type": "array",
                    "items": {
                      "type": "number"
                    }
                  },
                  "description": "Vector data to search "
                }
              }
            },
            "SearchResponse": {
              "type": "object",
              "description": "Response received after a successful search",
              "properties": {
                "status": {
                  "type": "object",
                  "description": "Status of the search operation",
                  "properties": {
                    "error_code": {
                      "type": "integer",
                      "description": "If the search failed, a code indicating the failure;"
                    },
                  }
                },
                "results": {
                  "type": "object",
                  "description": "The results from the search",
                  "properties": {
                    "num_queries": {
                      "type": "integer",
                      "description": "The number of queries made"
                    },
                    "scores": {
                      "type": "array",
                      "items": {
                        "type": "number"
                      },
                      "description": "Scores of the search results"
                    },
                    "ids": {
                      "type": "object",
                      "description": "IDs of the search results",
                      "properties": {
                        "IdField": {
                          "type": "object",
                          "properties": {
                            "IntId": {
                              "type": "object",
                              "properties": {
                                "data": {
                                  "type": "array",
                                  "items": {
                                    "type": "integer"
                                  },
                                  "description": "Array of integer IDs"
                                }
                              }
                            }
                          }
                        }
                      }
                    }
                  }
                },
                "collection_name": {
                  "type": "string",
                  "description": "The name of the collection searched in"
                }
              }
            },
            "FieldData": {
              "type": "object",
              "description": "Data related to a specific field",
              "properties": {
                "type": {
                  "type": "integer",
                  "description": "The type of the field"
                },
                "field_name": {
                  "type": "string",
                  "description": "The name of the field"
                },
              },
                "field_id": {
                  "type": "integer",
                  "description": "The ID of the field"
                }
              }
            }
          }
        }
      }

## openapi spec for ibm-rpa

    answer: |
      {
        "openapi": "3.0.3",
        "info": {
          "title": "Swagger IBM RPA Asynch Server",
          "description": "This is a Swagger representing IBM RPA Server for Asynch",
          "version": "1.0.11"
        },
        "externalDocs": {
          "description": "Find out more about Swagger",
          "url": "http://swagger.io"
        },
        "servers": [
          {
            "url": "https://{region}.functions.cloud.ibm.com/api/v1/web/{cloud_function_id}/default",
            "variables": {
              "region": {
                "default": "us-south",
                "description": "This value is based on which region your cloud functions are located."
              },
              "cloud_function_id": {
                "default": "cloud_function_id"
              }
            }
          }
        ],
        "paths": {
          "/Start%20RPA%20Asynchronously.json": {
            "post": {
              "parameters": [
                {
                  "name": "guid",
                  "in": "query",
                  "required": true,
                  "description": "a unique identifier representing the cloud function namespace",
                  "schema": {
                    "type": "string"
                  }
                }
              ],
              "summary": "Insert Item into IBM RPA",
              "description": "Insert Item into IBM RPA to kick off an instance",
              "operationId": "insertItem",
              "requestBody": {
                "description": "Payload to insert items into IBM RPA by means of Cloud Function",
                "content": {
                  "application/json": {
                    "schema": {
                      "$ref": "#/components/schemas/InsertItem"
                    }
                  }
                }
              },
              "responses": {
                "200": {
                  "description": "Successful operation",
                  "content": {
                    "application/json": {
                      "schema": {
                        "$ref": "#/components/schemas/InstanceOutput"
                      }
                    }
                  }
                },
                "405": {
                  "description": "Invalid input"
                }
              }
            }
          },
          "/Get%20RPA%20Asynchronous%20Status.json": {
            "post": {
              "parameters": [
                {
                  "name": "guid",
                  "in": "query",
                  "required": true,
                  "description": "a unique identifier representing the cloud function namespace",
                  "schema": {
                    "type": "string"
                  }
                }
              ],
              "summary": "Get RPA Status",
              "description": "Get Status of an RPA Instance",
              "operationId": "getStatusOfRPAInstance",
              "requestBody": {
                "description": "Payload to get status of an item in IBM RPA by means of Cloud Function",
                "content": {
                  "application/json": {
                    "schema": {
                      "$ref": "#/components/schemas/InstanceStatus"
                    }
                  }
                }
              },
              "responses": {
                "200": {
                  "description": "Successful operation",
                  "content": {
                    "application/json": {
                      "schema": {
                        "$ref": "#/components/schemas/StatusOutput"
                      }
                    }
                  }
                },
                "405": {
                  "description": "Invalid input"
                }
              }
            }
          }
        },
        "components": {
          "schemas": {
            "InsertItem": {
              "type": "object",
              "properties": {
                "rpaInputParam1": {
                  "type": "string",
                  "description": "RPA First input Value"
                },
                "rpaInputParam2": {
                  "type": "string",
                  "description": "RPA Second input Value"
                }
              }
            },
            "InstanceStatus": {
              "type": "object",
              "properties": {
                "instanceID": {
                  "type": "string",
                  "description": "ID of Item"
                }
              }
            },
            "InstanceOutput": {
              "type": "object",
              "properties": {
                "rpaInstanceID": {
                  "type": "string",
                  "example": "0bO0vKrd00cTYYBZe0Po8ZxPevFiZXa4TrLj0UwZh8kSWAZNoU6bwoo-heffvABDspPvryNzMKzyU9waPrEuctkAhxiEgsRMqPzWVyZa1a-pMsqIXtZnb_F1KPi44JpkXfQyRe0QqFWn8a-IaeH2Ld874kWjg2Av0BrHKZ9SxGc4OgRNuTZDBxMKIg-27eFe79SxP870Pt-6jtNsQRpRuKtISdfRX9vktC0qa2Day8mmso_lHXhc5CowEVisUQ0eXLqLJM2Eb8YATyEIJfYkUNxgzMXue7HJ1u8lObGPlAdVO7DdIcoOX4MbSQjE4XLbIG5VY05BJT4Jys0c-c5yuVlXezLzDlU9e4Z7LVpywz7ZYwxCJKzMtEVZYO--YLGR8543G3AzH8CeZASHh5aTg8a-J3LVpW1tEbjspjCJXLW76pHsUhi3oCDbBqHxKLVIpuhJLcXErFh"
                }
              }
            },
            "StatusOutput": {
              "type": "object",
              "properties": {
                "rpaInstanceStatus": {
                  "type": "object",
                  "properties": {
                    "rpaOutputVariable1": {
                      "type": "string"
                    },
                    "rpaOutputVariable2": {
                      "type": "string"
                    }
                  }
                }
              }
            }
          }
        }
      }

##openapi spec for huggigface

    answer: |
      {
        "openapi": "3.0.3",
        "info": {
          "version": "1.0.0",
          "title": "Hugging Face Hub Embeddings API",
          "description": "Unofficial simplified OpenAPI specification for generating embeddings using Hugging Face Hub."
        },
        "servers": [
          {
            "url": "https://api-inference.huggingface.co"
          }
        ],
        "paths": {
          "/pipeline/feature-extraction/sentence-transformers/{model_id}": {
            "post": {
              "summary": "Generate embeddings using a sentence-transformers model",
              "parameters": [
                {
                  "name": "model_id",
                  "in": "path",
                  "description": "ID of the sentence-transformers model to use for generating embeddings.",
                  "example": "all-MiniLM-L6-v2",
                  "required": true,
                  "schema": {
                    "type": "string"
                  }
                }
              ],
              "requestBody": {
                "required": true,
                "content": {
                  "application/json": {
                    "schema": {
                      "$ref": "#/components/schemas/EmbeddingInput"
                    }
                  }
                }
              },
              "responses": {
                "200": {
                  "description": "Successful operation",
                  "content": {
                    "application/json": {
                      "schema": {
                        "$ref": "#/components/schemas/EmbeddingOutput"
                      }
                    }
                  }
                },
                "400": {
                  "description": "Invalid input"
                }
              }
            }
          }
        },
        "components": {
          "securitySchemes": {
            "bearerAuth": {
              "type": "http",
              "scheme": "bearer"
            }
          },
          "schemas": {
            "EmbeddingInput": {
              "type": "object",
              "properties": {
                "inputs": {
                  "type": "array",
                  "items": {
                    "type": "string"
                  },
                  "description": "An array of input text to generate embeddings for."
                },
                "options": {
                  "type": "object",
                  "properties": {
                    "wait_for_model": {
                      "type": "boolean",
                      "description": "Flag indicating whether to wait for the model to be downloaded"
                    }
                  },
                  "description": "Additional options for generating embeddings."
                }
              },
              "required": ["inputs"],
              "example": {
                "inputs": ["How do I start a purchase order?"],
                "options": {
                  "wait_for_model": true
                }
              }
            },
            "EmbeddingOutput": {
              "type": "array",
              "items": {
                "type": "array",
                "items": {
                  "type": "number"
                }
              },
              "description": "The generated embeddings."
            }
          }
        }
      }

## openapi spec for watson discovery

    answer: |
      {
        "openapi": "3.0.1",
        "info": {
          "title": "IBM Watson Discovery query endpoint",
          "description": "IBM Watson Discovery simplified API spec for querying.",
          "version": "1.1"
        },
        "servers": [
          {
            "url": "https://{discovery_url}",
            "description": "The Watson Discovery URL",
            "variables": {
              "discovery_url": {
                "default": "api.us-south.discovery.watson.cloud.ibm.com/instances/12345-6789",
                "description": "The portions of the Watson Discovery URL that follow https://"
              }
            }
          }
        ],
        "security": [
          {
            "bearerAuth": []
          },
          {
            "basicAuth": []
          }
        ],
        "paths": {
          "/v2/projects/{project_id}/query": {
            "post": {
              "operationId": "query",
              "summary": "Query a project",
              "description": "Search your data by submitting queries that are written",
              "tags": [
                "Queries"
              ],
              "parameters": [
                {
                  "name": "version",
                  "in": "query",
                  "description": "Release date of the version of the API you want to use.",
                  "required": true,
                  "schema": {
                    "type": "string"
                  }
                },
                {
                  "name": "project_id",
                  "in": "path",
                  "description": "The ID of the project. ",
                  "required": true,
                  "schema": {
                    "type": "string",
                    "minLength": 1,
                    "maxLength": 255,
                    "pattern": "^[a-zA-Z0-9_-]*$"
                  }
                }
              ],
              "responses": {
                "200": {
                  "description": "Query executed successfully.",
                  "content": {
                    "application/json": {
                      "schema": {
                        "$ref": "#/components/schemas/QueryResponse"
                      }
                    }
                  }
                }
              }
            }
          }
        },
        "components": {
          "securitySchemes": {
            "basicAuth": {
              "type": "http",
              "scheme": "basic"
            },
            "bearerAuth": {
              "type": "http",
              "scheme": "bearer"
            }
          },
          "parameters": {
            "collectionIdsParam": {
              "name": "collection_ids",
              "in": "query",
              "description": "Comma separated list of the collection IDs. ",
              "schema": {
                "type": "array",
                "items": {
                  "type": "string"
                }
              }
            },
            "collectionIdParam": {
              "name": "collection_id",
              "in": "path",
              "description": "The ID of the collection.",
              "required": true,
              "schema": {
                "type": "string",
                "minLength": 1,
                "maxLength": 255,
                "pattern": "^[a-zA-Z0-9_-]*$"
              }
            },
            "documentIdParam": {
              "name": "document_id",
              "in": "path",
              "description": "The ID of the document.",
              "required": true,
              "schema": {
                "type": "string",
                "minLength": 1,
                "maxLength": 255,
                "pattern": "^[a-zA-Z0-9_-]*$"
              }
            },
            "acFieldParam": {
              "name": "field",
              "in": "query",
              "description": "The field in the result documents that autocompletion suggestions are identified from.",
              "schema": {
                "type": "string"
              }
            },
            "docCountParam": {
              "name": "count",
              "in": "query",
              "description": "The maximum number of documents to return.",
              "schema": {
                "type": "integer",
                "default": 1000
              }
            }
          },
          "requestBodies": {},
          "schemas": {
            "TableElementLocation": {
              "type": "object",
              "required": [
                "begin",
                "end"
              ],
              "description": "The numeric location of the identified element in the document",
              "properties": {
                "begin": {
                  "description": "The element's `begin` index.",
                  "type": "integer",
                  "format": "int64"
                },
                "end": {
                  "description": "The element's `end` index.",
                  "type": "integer",
                  "format": "int64"
                }
              }
            },
          }
        }
      }

## openapi spec for mail sending

      {
          "openapi": "3.0.0",
          "info": {
            "title": "Email Service API",
            "version": "1.0.0"
          },
          "servers": [
            {
              "url": "https://{host}:{port}",
              "description": "The address of SMTP extension server",
              "variables": {
                "host": {
                  "default": "",
                  "description": "Hostname of the API server"
                },
                "port": {
                  "default": "443",
                  "description": "Port of the API server"
                }
              }
            }
          ],
          "paths": {
            "/send": {
              "post": {
                "summary": "Send an email",
                "requestBody": {
                  "required": true,
                  "content": {
                    "application/json": {
                      "schema": {
                        "type": "object",
                        "properties": {
                          "receiver": {
                            "type": "string"
                          },
                          "subject": {
                            "type": "string"
                          },
                          "body": {
                            "type": "string"
                          }
                        },
                        "required": [
                          "receiver",
                          "subject",
                          "body"
                        ]
                      }
                    }
                  }
                },
                "responses": {
                  "200": {
                    "description": "Email sent successfully",
                    "content": {
                      "application/json": {
                        "schema": {
                          "type": "object",
                          "properties": {
                            "message": {
                              "type": "string"
                            },
                            "success": {
                              "type": "boolean"
                            }
                          }
                        }
                      }
                    }
                  },
                  "401": {
                    "description": "Unauthorized - Bearer token is missing or invalid"
                  },
                  "500": {
                    "description": "An error occurred while sending the email"
                  }
                },
                "security": [
                  {
                    "BearerAuth": []
                  }
                ]
              }
            }
          },
          "security": [
            {
              "BearerAuth": []
            }
          ],
          "components": {
            "securitySchemes": {
              "BearerAuth": {
                "type": "http",
                "scheme": "bearer"
              }
            }
          }
      }

## openapi spec for twilio

      {
         "openapi": "3.0.0",
         "info": {
           "title": "Twilio API",
           "version": "1.0.0",
           "description": "API for making calls using Twilio",
           "x-ibm-annotations": "true",
           "x-ibm-application-name": "Twilio API",
           "x-ibm-application-id": "twilio-api",
           "x-ibm-skill-type": "imported",
           "x-ibm-application-icon": "<svg xmlns='http://www.w3.org/2000/svg'></svg>"
         },
         "servers": [
           {
             "url": "https://api.twilio.com"
           }
         ],
         "paths": {
           "/2010-04-01/Accounts/{AccountSid}/Calls.json": {
             "post": {
               "tags": ["calls"],
               "summary": "Make a call",
               "description": "Initiate a new outgoing call",
               "operationId": "makeCall",
               "parameters": [
                 {
                   "name": "AccountSid",
                   "in": "path",
                   "required": true,
                   "schema": {
                     "type": "string"
                   }
                 }
               ],
               "requestBody": {
                 "description": "Call details",
                 "required": true,
                 "content": {
                   "application/x-www-form-urlencoded": {
                     "schema": {
                       "type": "object",
                       "properties": {
                         "Url": {
                           "type": "string",
                           "description": "The URL that returns the TwiML instructions for the call.",
                           "example": "http://demo.twilio.com/docs/voice.xml"
                         },
                         "To": {
                           "type": "string",
                           "description": "The phone number to call.",
                           "example": "+14155551212"
                         },
                         "From": {
                           "type": "string",
                           "description": "The phone number to use as the caller ID.",
                           "example": "+15017122661"
                         }
                       },
                       "required": ["Url", "To", "From"]
                     }
                   }
                 }
               },
               "responses": {
                 "200": {
                   "description": "Call initiated successfully",
                   "content": {
                     "application/json": {
                       "schema": {
                         "type": "object",
                         "properties": {
                           "sid": {
                             "type": "string"
                           },
                           "status": {
                             "type": "string"
                           }
                         }
                       }
                     }
                   }
                 },
                 "400": {
                   "description": "Invalid request"
                 }
               },
               "security": [
                 {
                   "basic_auth": []
                 }
               ]
             }
           }
         },
         "components": {
           "securitySchemes": {
             "basic_auth": {
               "type": "http",
               "scheme": "basic"
             }
           }
         }
       }