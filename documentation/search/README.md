# k8or Portal Seach Image Process

**Component C8**, the **k8or Portal**, is deployed on an EC2 instance designated as the `k8or Portal node`. This node functions similarly to a node within an EKS or K3s cluster, hosting multiple microservices (MSVs). Each functional process (`upload`, `search`, `transfer`, `deploy`, `scan`, `chart`, `delete`, and future additions) is handled by dedicated microservices. Processes requiring a user interface have both frontend (FMS) and backend (BMS) microservices, while backend-only processes utilize only Backend Microservices.

### Process ID (PID) Structure

Each step in the process flows is assigned a unique Process ID (PID) to facilitate tracking and analysis. The PID follows a structured format:

```
PID: <start-resource>-<end-resource>-e<execution_order>
```

1.  **Resource Identifiers:** The PID identifies the starting and ending resources involved in a specific process step. Resources can be either microservices (MSVs, e.g., `fms09`, `bms12`) backend resources (e.g., `bre03`, `bre06`), or a Frontend User (e.g., `usr03`, `usr06`).
2.  **Component ID Prefix:** Both the start and end resource identifiers are prefixed with a component ID (e.g., `c2`, `c4`, `c8`) to specify the component to which the resource belongs. For example, `c8fms09` indicates that `fms09` is part of component `c8`.
3.  **Execution Order:** The `e<execution_order>` component of the PID indicates the sequence of execution. The `e` prefix denotes `execution`, and the subsequent number represents the step number in increments of 5 (e.g., `e05`, `e10`, `e15`, `e20`). This allows for easy identification of the order in which the processes occur.  For instance, `e05` represents the first step, `e10` the second, and so on.

# Process Flows

### Diagram: [View](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/tree/k8or-dev/search-dir/k8or-portal-search-v0-0-03-dia-k8d.drawio.pdf)

![Alt text](https://github.com/k8or-orbit-aws/k8or-orbit-doc-0103-rep-k8d-aws/blob/k8or-dev/search-dir/k8or-portal-search-v0-0-03-dia-k8d.jpg)

### 1. c2usr06-c8fms06-e05: The **`c2usr06` Frontend User** navigates to the Portal Search UI.
* **Endpoint:** https://orbit.k8or.com/search/
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`

### 2. c8fms06-c8bms18-e10: The **`Portal Upload Board` Frontend Microservice** `c8fms06` communicates with the **`Portal Search Logic` Backend Microservice** `c8bms18` to retrieve posts from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/posts?per_page=5&page=1&_embed`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 70,
        "date": "2025-02-07T12:14:27",
        "date_gmt": "2025-02-07T12:14:27",
        "guid": {
            "rendered": "http:\/\/orbit.k8or.com\/viewport\/?p=70"
        },
        "modified": "2025-02-14T14:53:52",
        "modified_gmt": "2025-02-14T14:53:52",
        "slug": "the-grinch-2018",
        "status": "publish",
        "type": "post",
        "link": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/",
        "title": {
            "rendered": "The Grinch (2018)"
        },
        "content": {
            "rendered": "<p>The Grinch, also known as Dr. Seuss&#8217; The Grinch, is a 2018 American animated Christmas comedy film produced by Universal Pictures and Illumination, and distributed by Universal. The third screen adaptation of Dr. Seuss&#8217; 1957 book How the Grinch Stole Christmas!, following the 1966 television special and the 2000 live-action feature-length film, it is Illumination&#8217;s second Dr. Seuss film adaptation, after The Lorax in 2012. The plot follows the Grinch, who plans to stop Whoville&#8217;s Christmas celebration by stealing all the town&#8217;s decorations and gifts, with his pet dog Max.<\/p>\n",
            "protected": false
        },
        "excerpt": {
            "rendered": "<p>The Grinch, also known as Dr. Seuss&#8217; The Grinch, is a 2018 American animated Christmas comedy film produced by Universal Pictures and Illumination, and distributed by Universal. The third screen&#8230; <a class=\"read-more\" href=\"http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/\">Read More<\/a><\/p>\n",
            "protected": false
        },
        "author": 3,
        "featured_media": 74,
        "comment_status": "open",
        "ping_status": "open",
        "sticky": false,
        "template": "",
        "format": "standard",
        "meta": {
            "_edit_lock": [
                "1739630194:1"
            ],
            "_edit_last": [
                "1"
            ],
            "_thumbnail_id": [
                "74"
            ],
            "publication": [
                "Empower99"
            ],
            "_publication": [
                "field_67a34d2fe986e"
            ],
            "preview": [
                "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/"
            ],
            "_preview": [
                "field_67a34d3be986f"
            ],
            "live_preview": [
                ""
            ],
            "_live_preview": [
                "field_67a34d46e9870"
            ],
            "website": [
                "http:\/\/orbit.k8or.com\/"
            ],
            "_website": [
                "field_67a34d4ee9871"
            ],
            "website-main-article": [
                "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/"
            ],
            "_website-main-article": [
                "field_67a34d52e9872"
            ],
            "plan": [
                "coming soon"
            ],
            "_plan": [
                "field_67a34d5ae9873"
            ],
            "recommender": [
                "None"
            ],
            "_recommender": [
                "field_67a34d69e9874"
            ],
            "recommender_url": [
                ""
            ],
            "_recommender_url": [
                "field_67a34d6ce9875"
            ],
            "preview_button_text": [
                "Reveal the Data"
            ],
            "_preview_button_text": [
                "field_67a34d74e9876"
            ],
            "start_date": [
                "20250207"
            ],
            "_start_date": [
                "field_67a34d82e9877"
            ],
            "version": [
                "v0-0-01"
            ],
            "_version": [
                "field_67a37259b0c3a"
            ],
            "scanned": [
                "No"
            ],
            "_scanned": [
                "field_67a372a802406"
            ],
            "part": [
                "4562"
            ],
            "_part": [
                "field_67a3735718fc6"
            ],
            "image_status": [
                "Transferred"
            ],
            "_image_status": [
                "field_67a37d949ec30"
            ],
            "imageuid": [
                "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
            ],
            "_imageuid": [
                "field_67aa0c4581976"
            ],
            "environment": [
                "Testing"
            ],
            "_environment": [
                "field_67aa0c5481977"
            ]
        },
        "categories": [
            4
        ],
        "tags": [
            5
        ],
        "variable": [
            12
        ],
        "part": [],
        "class_list": [
            "post-70",
            "post",
            "type-post",
            "status-publish",
            "format-standard",
            "has-post-thumbnail",
            "hentry",
            "category-music",
            "tag-development",
            "variable-part-number"
        ],
        "acf": {
            "publication": "Empower99",
            "preview": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/",
            "live_preview": "",
            "website": "http:\/\/orbit.k8or.com\/",
            "website-main-article": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/",
            "plan": "coming soon",
            "recommender": "None",
            "recommender_url": "",
            "preview_button_text": "Reveal the Data",
            "start_date": "20250207",
            "version": "v0-0-01",
            "scanned": "No",
            "part": "4562",
            "image_status": "Transferred",
            "imageuid": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
            "environment": "Testing"
        },
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/70",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/post"
                }
            ],
            "author": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/3"
                }
            ],
            "replies": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=70"
                }
            ],
            "version-history": [
                {
                    "count": 7,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/70\/revisions"
                }
            ],
            "predecessor-version": [
                {
                    "id": 89,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/70\/revisions\/89"
                }
            ],
            "wp:featuredmedia": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/74"
                }
            ],
            "wp:attachment": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media?parent=70"
                }
            ],
            "wp:term": [
                {
                    "taxonomy": "category",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories?post=70"
                },
                {
                    "taxonomy": "post_tag",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags?post=70"
                },
                {
                    "taxonomy": "variable",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable?post=70"
                },
                {
                    "taxonomy": "part",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part?post=70"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        },
        "_embedded": {
            "author": [
                {
                    "id": 3,
                    "name": "Madame Coco",
                    "url": "http:\/\/www.k8or.com",
                    "description": "",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/author\/madamecoco\/",
                    "slug": "madamecoco",
                    "avatar_urls": {
                        "24": "https:\/\/secure.gravatar.com\/avatar\/b1b1973ced7695444eb70bab41e182fd?s=24&d=mm&r=g",
                        "48": "https:\/\/secure.gravatar.com\/avatar\/b1b1973ced7695444eb70bab41e182fd?s=48&d=mm&r=g",
                        "96": "https:\/\/secure.gravatar.com\/avatar\/b1b1973ced7695444eb70bab41e182fd?s=96&d=mm&r=g"
                    },
                    "acf": [],
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/3",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users"
                            }
                        ]
                    }
                }
            ],
            "wp:featuredmedia": [
                {
                    "id": 74,
                    "date": "2025-02-07T12:18:28",
                    "slug": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90",
                    "type": "attachment",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90\/",
                    "title": {
                        "rendered": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90"
                    },
                    "author": 3,
                    "featured_media": 0,
                    "acf": [],
                    "caption": {
                        "rendered": ""
                    },
                    "alt_text": "",
                    "media_type": "image",
                    "mime_type": "image\/jpeg",
                    "media_details": {
                        "width": 525,
                        "height": 526,
                        "file": "2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg",
                        "filesize": 69295,
                        "sizes": {
                            "medium": {
                                "file": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-300x300.jpeg",
                                "width": 300,
                                "height": 300,
                                "filesize": 18438,
                                "mime_type": "image\/jpeg",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-300x300.jpeg"
                            },
                            "thumbnail": {
                                "file": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-150x150.jpeg",
                                "width": 150,
                                "height": 150,
                                "filesize": 6840,
                                "mime_type": "image\/jpeg",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-150x150.jpeg"
                            },
                            "full": {
                                "file": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg",
                                "width": 525,
                                "height": 526,
                                "mime_type": "image\/jpeg",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg"
                            }
                        },
                        "image_meta": {
                            "aperture": "0",
                            "credit": "",
                            "camera": "",
                            "caption": "",
                            "created_timestamp": "0",
                            "copyright": "",
                            "focal_length": "0",
                            "iso": "0",
                            "shutter_speed": "0",
                            "title": "",
                            "orientation": "0",
                            "keywords": []
                        }
                    },
                    "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg",
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/74",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media"
                            }
                        ],
                        "about": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/attachment"
                            }
                        ],
                        "author": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/3"
                            }
                        ],
                        "replies": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=74"
                            }
                        ]
                    }
                }
            ],
            "wp:term": [
                [
                    {
                        "id": 4,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/music\/",
                        "name": "Music",
                        "slug": "music",
                        "taxonomy": "category",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/4",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=4"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 5,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/tag\/development\/",
                        "name": "Development",
                        "slug": "development",
                        "taxonomy": "post_tag",
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags\/5",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/post_tag"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?tags=5"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 12,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/part-number\/",
                        "name": "Part number",
                        "slug": "part-number",
                        "taxonomy": "variable",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/12",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=12"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                []
            ]
        }
    },
    {
        "id": 53,
        "date": "2025-02-07T11:51:57",
        "date_gmt": "2025-02-07T11:51:57",
        "guid": {
            "rendered": "http:\/\/orbit.k8or.com\/viewport\/?p=53"
        },
        "modified": "2025-02-14T14:53:48",
        "modified_gmt": "2025-02-14T14:53:48",
        "slug": "trolls-world-tour-2020",
        "status": "publish",
        "type": "post",
        "link": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/",
        "title": {
            "rendered": "Trolls World Tour (2020)"
        },
        "content": {
            "rendered": "<p>In the techno kingdom, King Trollex&#8217;s tribe of Techno Trolls are attending a rave, when Hard Rock Trolls led by Queen Barb arrive and use weaponized guitars to destroy everything. Barb demands Trollex surrender and hand over his &#8220;string&#8221;. In Pop Village, two years after making peace with the Bergens, Poppy, still trying to get used to being queen of the Pop Trolls, receives a letter from Barb, inviting her to bring her &#8220;string&#8221; to unite the trolls.<\/p>\n",
            "protected": false
        },
        "excerpt": {
            "rendered": "<p>In the techno kingdom, King Trollex&#8217;s tribe of Techno Trolls are attending a rave, when Hard Rock Trolls led by Queen Barb arrive and use weaponized guitars to destroy everything&#8230;. <a class=\"read-more\" href=\"http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/\">Read More<\/a><\/p>\n",
            "protected": false
        },
        "author": 2,
        "featured_media": 56,
        "comment_status": "open",
        "ping_status": "open",
        "sticky": false,
        "template": "",
        "format": "standard",
        "meta": {
            "_edit_lock": [
                "1739546593:1"
            ],
            "_edit_last": [
                "1"
            ],
            "_thumbnail_id": [
                "56"
            ],
            "publication": [
                "Empower99"
            ],
            "_publication": [
                "field_67a34d2fe986e"
            ],
            "preview": [
                "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/"
            ],
            "_preview": [
                "field_67a34d3be986f"
            ],
            "live_preview": [
                ""
            ],
            "_live_preview": [
                "field_67a34d46e9870"
            ],
            "website": [
                "http:\/\/orbit.k8or.com\/"
            ],
            "_website": [
                "field_67a34d4ee9871"
            ],
            "website-main-article": [
                "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/"
            ],
            "_website-main-article": [
                "field_67a34d52e9872"
            ],
            "plan": [
                "coming soon"
            ],
            "_plan": [
                "field_67a34d5ae9873"
            ],
            "recommender": [
                "None"
            ],
            "_recommender": [
                "field_67a34d69e9874"
            ],
            "recommender_url": [
                ""
            ],
            "_recommender_url": [
                "field_67a34d6ce9875"
            ],
            "preview_button_text": [
                "Reveal the Data"
            ],
            "_preview_button_text": [
                "field_67a34d74e9876"
            ],
            "start_date": [
                "20250207"
            ],
            "_start_date": [
                "field_67a34d82e9877"
            ],
            "version": [
                "v0-0-01"
            ],
            "_version": [
                "field_67a37259b0c3a"
            ],
            "scanned": [
                "No"
            ],
            "_scanned": [
                "field_67a372a802406"
            ],
            "part": [
                "7893"
            ],
            "_part": [
                "field_67a3735718fc6"
            ],
            "image_status": [
                "Transferred"
            ],
            "_image_status": [
                "field_67a37d949ec30"
            ],
            "imageuid": [
                "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
            ],
            "_imageuid": [
                "field_67aa0c4581976"
            ],
            "environment": [
                "Testing"
            ],
            "_environment": [
                "field_67aa0c5481977"
            ]
        },
        "categories": [
            3
        ],
        "tags": [
            5
        ],
        "variable": [
            12
        ],
        "part": [],
        "class_list": [
            "post-53",
            "post",
            "type-post",
            "status-publish",
            "format-standard",
            "has-post-thumbnail",
            "hentry",
            "category-movie",
            "tag-development",
            "variable-part-number"
        ],
        "acf": {
            "publication": "Empower99",
            "preview": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/",
            "live_preview": "",
            "website": "http:\/\/orbit.k8or.com\/",
            "website-main-article": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/",
            "plan": "coming soon",
            "recommender": "None",
            "recommender_url": "",
            "preview_button_text": "Reveal the Data",
            "start_date": "20250207",
            "version": "v0-0-01",
            "scanned": "No",
            "part": "7893",
            "image_status": "Transferred",
            "imageuid": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
            "environment": "Testing"
        },
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/53",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/post"
                }
            ],
            "author": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/2"
                }
            ],
            "replies": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=53"
                }
            ],
            "version-history": [
                {
                    "count": 16,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/53\/revisions"
                }
            ],
            "predecessor-version": [
                {
                    "id": 80,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/53\/revisions\/80"
                }
            ],
            "wp:featuredmedia": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/56"
                }
            ],
            "wp:attachment": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media?parent=53"
                }
            ],
            "wp:term": [
                {
                    "taxonomy": "category",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories?post=53"
                },
                {
                    "taxonomy": "post_tag",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags?post=53"
                },
                {
                    "taxonomy": "variable",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable?post=53"
                },
                {
                    "taxonomy": "part",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part?post=53"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        },
        "_embedded": {
            "author": [
                {
                    "id": 2,
                    "name": "James Bond",
                    "url": "http:\/\/www.k8or.com",
                    "description": "",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/author\/jamesbond\/",
                    "slug": "jamesbond",
                    "avatar_urls": {
                        "24": "https:\/\/secure.gravatar.com\/avatar\/468350c2f7811952df086715ea95e3c9?s=24&d=mm&r=g",
                        "48": "https:\/\/secure.gravatar.com\/avatar\/468350c2f7811952df086715ea95e3c9?s=48&d=mm&r=g",
                        "96": "https:\/\/secure.gravatar.com\/avatar\/468350c2f7811952df086715ea95e3c9?s=96&d=mm&r=g"
                    },
                    "acf": [],
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/2",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users"
                            }
                        ]
                    }
                }
            ],
            "wp:featuredmedia": [
                {
                    "id": 56,
                    "date": "2025-02-07T11:53:51",
                    "slug": "trolls-logo1-1024x1024",
                    "type": "attachment",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/trolls-logo1-1024x1024\/",
                    "title": {
                        "rendered": "TROLLS-LOGO1-1024&#215;1024"
                    },
                    "author": 2,
                    "featured_media": 0,
                    "acf": [],
                    "caption": {
                        "rendered": ""
                    },
                    "alt_text": "",
                    "media_type": "image",
                    "mime_type": "image\/webp",
                    "media_details": {
                        "width": 1024,
                        "height": 1024,
                        "file": "2025\/02\/TROLLS-LOGO1-1024x1024-1.webp",
                        "filesize": 38734,
                        "sizes": {
                            "medium": {
                                "file": "TROLLS-LOGO1-1024x1024-1-300x300.webp",
                                "width": 300,
                                "height": 300,
                                "filesize": 6562,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1-300x300.webp"
                            },
                            "thumbnail": {
                                "file": "TROLLS-LOGO1-1024x1024-1-150x150.webp",
                                "width": 150,
                                "height": 150,
                                "filesize": 2944,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1-150x150.webp"
                            },
                            "medium_large": {
                                "file": "TROLLS-LOGO1-1024x1024-1-768x768.webp",
                                "width": 768,
                                "height": 768,
                                "filesize": 20020,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1-768x768.webp"
                            },
                            "full": {
                                "file": "TROLLS-LOGO1-1024x1024-1.webp",
                                "width": 1024,
                                "height": 1024,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1.webp"
                            }
                        },
                        "image_meta": {
                            "aperture": "0",
                            "credit": "",
                            "camera": "",
                            "caption": "",
                            "created_timestamp": "0",
                            "copyright": "",
                            "focal_length": "0",
                            "iso": "0",
                            "shutter_speed": "0",
                            "title": "",
                            "orientation": "0",
                            "keywords": []
                        }
                    },
                    "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1.webp",
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/56",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media"
                            }
                        ],
                        "about": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/attachment"
                            }
                        ],
                        "author": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/2"
                            }
                        ],
                        "replies": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=56"
                            }
                        ]
                    }
                }
            ],
            "wp:term": [
                [
                    {
                        "id": 3,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/movie\/",
                        "name": "Movie",
                        "slug": "movie",
                        "taxonomy": "category",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/3",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=3"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 5,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/tag\/development\/",
                        "name": "Development",
                        "slug": "development",
                        "taxonomy": "post_tag",
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags\/5",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/post_tag"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?tags=5"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 12,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/part-number\/",
                        "name": "Part number",
                        "slug": "part-number",
                        "taxonomy": "variable",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/12",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=12"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                []
            ]
        }
    }
]
```

### 3. c8bms18-c32bre03-e15: The **`Portal Search Logic` Backend Microservice** `c8bms18` retrieves posts from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/posts?per_page=5&page=1&_embed`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 70,
        "date": "2025-02-07T12:14:27",
        "date_gmt": "2025-02-07T12:14:27",
        "guid": {
            "rendered": "http:\/\/orbit.k8or.com\/viewport\/?p=70"
        },
        "modified": "2025-02-14T14:53:52",
        "modified_gmt": "2025-02-14T14:53:52",
        "slug": "the-grinch-2018",
        "status": "publish",
        "type": "post",
        "link": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/",
        "title": {
            "rendered": "The Grinch (2018)"
        },
        "content": {
            "rendered": "<p>The Grinch, also known as Dr. Seuss&#8217; The Grinch, is a 2018 American animated Christmas comedy film produced by Universal Pictures and Illumination, and distributed by Universal. The third screen adaptation of Dr. Seuss&#8217; 1957 book How the Grinch Stole Christmas!, following the 1966 television special and the 2000 live-action feature-length film, it is Illumination&#8217;s second Dr. Seuss film adaptation, after The Lorax in 2012. The plot follows the Grinch, who plans to stop Whoville&#8217;s Christmas celebration by stealing all the town&#8217;s decorations and gifts, with his pet dog Max.<\/p>\n",
            "protected": false
        },
        "excerpt": {
            "rendered": "<p>The Grinch, also known as Dr. Seuss&#8217; The Grinch, is a 2018 American animated Christmas comedy film produced by Universal Pictures and Illumination, and distributed by Universal. The third screen&#8230; <a class=\"read-more\" href=\"http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/\">Read More<\/a><\/p>\n",
            "protected": false
        },
        "author": 3,
        "featured_media": 74,
        "comment_status": "open",
        "ping_status": "open",
        "sticky": false,
        "template": "",
        "format": "standard",
        "meta": {
            "_edit_lock": [
                "1739630194:1"
            ],
            "_edit_last": [
                "1"
            ],
            "_thumbnail_id": [
                "74"
            ],
            "publication": [
                "Empower99"
            ],
            "_publication": [
                "field_67a34d2fe986e"
            ],
            "preview": [
                "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/"
            ],
            "_preview": [
                "field_67a34d3be986f"
            ],
            "live_preview": [
                ""
            ],
            "_live_preview": [
                "field_67a34d46e9870"
            ],
            "website": [
                "http:\/\/orbit.k8or.com\/"
            ],
            "_website": [
                "field_67a34d4ee9871"
            ],
            "website-main-article": [
                "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/"
            ],
            "_website-main-article": [
                "field_67a34d52e9872"
            ],
            "plan": [
                "coming soon"
            ],
            "_plan": [
                "field_67a34d5ae9873"
            ],
            "recommender": [
                "None"
            ],
            "_recommender": [
                "field_67a34d69e9874"
            ],
            "recommender_url": [
                ""
            ],
            "_recommender_url": [
                "field_67a34d6ce9875"
            ],
            "preview_button_text": [
                "Reveal the Data"
            ],
            "_preview_button_text": [
                "field_67a34d74e9876"
            ],
            "start_date": [
                "20250207"
            ],
            "_start_date": [
                "field_67a34d82e9877"
            ],
            "version": [
                "v0-0-01"
            ],
            "_version": [
                "field_67a37259b0c3a"
            ],
            "scanned": [
                "No"
            ],
            "_scanned": [
                "field_67a372a802406"
            ],
            "part": [
                "4562"
            ],
            "_part": [
                "field_67a3735718fc6"
            ],
            "image_status": [
                "Transferred"
            ],
            "_image_status": [
                "field_67a37d949ec30"
            ],
            "imageuid": [
                "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
            ],
            "_imageuid": [
                "field_67aa0c4581976"
            ],
            "environment": [
                "Testing"
            ],
            "_environment": [
                "field_67aa0c5481977"
            ]
        },
        "categories": [
            4
        ],
        "tags": [
            5
        ],
        "variable": [
            12
        ],
        "part": [],
        "class_list": [
            "post-70",
            "post",
            "type-post",
            "status-publish",
            "format-standard",
            "has-post-thumbnail",
            "hentry",
            "category-music",
            "tag-development",
            "variable-part-number"
        ],
        "acf": {
            "publication": "Empower99",
            "preview": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/",
            "live_preview": "",
            "website": "http:\/\/orbit.k8or.com\/",
            "website-main-article": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/",
            "plan": "coming soon",
            "recommender": "None",
            "recommender_url": "",
            "preview_button_text": "Reveal the Data",
            "start_date": "20250207",
            "version": "v0-0-01",
            "scanned": "No",
            "part": "4562",
            "image_status": "Transferred",
            "imageuid": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
            "environment": "Testing"
        },
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/70",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/post"
                }
            ],
            "author": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/3"
                }
            ],
            "replies": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=70"
                }
            ],
            "version-history": [
                {
                    "count": 7,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/70\/revisions"
                }
            ],
            "predecessor-version": [
                {
                    "id": 89,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/70\/revisions\/89"
                }
            ],
            "wp:featuredmedia": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/74"
                }
            ],
            "wp:attachment": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media?parent=70"
                }
            ],
            "wp:term": [
                {
                    "taxonomy": "category",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories?post=70"
                },
                {
                    "taxonomy": "post_tag",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags?post=70"
                },
                {
                    "taxonomy": "variable",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable?post=70"
                },
                {
                    "taxonomy": "part",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part?post=70"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        },
        "_embedded": {
            "author": [
                {
                    "id": 3,
                    "name": "Madame Coco",
                    "url": "http:\/\/www.k8or.com",
                    "description": "",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/author\/madamecoco\/",
                    "slug": "madamecoco",
                    "avatar_urls": {
                        "24": "https:\/\/secure.gravatar.com\/avatar\/b1b1973ced7695444eb70bab41e182fd?s=24&d=mm&r=g",
                        "48": "https:\/\/secure.gravatar.com\/avatar\/b1b1973ced7695444eb70bab41e182fd?s=48&d=mm&r=g",
                        "96": "https:\/\/secure.gravatar.com\/avatar\/b1b1973ced7695444eb70bab41e182fd?s=96&d=mm&r=g"
                    },
                    "acf": [],
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/3",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users"
                            }
                        ]
                    }
                }
            ],
            "wp:featuredmedia": [
                {
                    "id": 74,
                    "date": "2025-02-07T12:18:28",
                    "slug": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90",
                    "type": "attachment",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/the-grinch-2018\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90\/",
                    "title": {
                        "rendered": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90"
                    },
                    "author": 3,
                    "featured_media": 0,
                    "acf": [],
                    "caption": {
                        "rendered": ""
                    },
                    "alt_text": "",
                    "media_type": "image",
                    "mime_type": "image\/jpeg",
                    "media_details": {
                        "width": 525,
                        "height": 526,
                        "file": "2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg",
                        "filesize": 69295,
                        "sizes": {
                            "medium": {
                                "file": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-300x300.jpeg",
                                "width": 300,
                                "height": 300,
                                "filesize": 18438,
                                "mime_type": "image\/jpeg",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-300x300.jpeg"
                            },
                            "thumbnail": {
                                "file": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-150x150.jpeg",
                                "width": 150,
                                "height": 150,
                                "filesize": 6840,
                                "mime_type": "image\/jpeg",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90-150x150.jpeg"
                            },
                            "full": {
                                "file": "c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg",
                                "width": 525,
                                "height": 526,
                                "mime_type": "image\/jpeg",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg"
                            }
                        },
                        "image_meta": {
                            "aperture": "0",
                            "credit": "",
                            "camera": "",
                            "caption": "",
                            "created_timestamp": "0",
                            "copyright": "",
                            "focal_length": "0",
                            "iso": "0",
                            "shutter_speed": "0",
                            "title": "",
                            "orientation": "0",
                            "keywords": []
                        }
                    },
                    "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/c2c97ef2-3ad7-452f-a572-9ab682d27e37-width936-quality90.jpeg",
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/74",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media"
                            }
                        ],
                        "about": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/attachment"
                            }
                        ],
                        "author": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/3"
                            }
                        ],
                        "replies": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=74"
                            }
                        ]
                    }
                }
            ],
            "wp:term": [
                [
                    {
                        "id": 4,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/music\/",
                        "name": "Music",
                        "slug": "music",
                        "taxonomy": "category",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/4",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=4"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 5,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/tag\/development\/",
                        "name": "Development",
                        "slug": "development",
                        "taxonomy": "post_tag",
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags\/5",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/post_tag"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?tags=5"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 12,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/part-number\/",
                        "name": "Part number",
                        "slug": "part-number",
                        "taxonomy": "variable",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/12",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=12"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                []
            ]
        }
    },
    {
        "id": 53,
        "date": "2025-02-07T11:51:57",
        "date_gmt": "2025-02-07T11:51:57",
        "guid": {
            "rendered": "http:\/\/orbit.k8or.com\/viewport\/?p=53"
        },
        "modified": "2025-02-14T14:53:48",
        "modified_gmt": "2025-02-14T14:53:48",
        "slug": "trolls-world-tour-2020",
        "status": "publish",
        "type": "post",
        "link": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/",
        "title": {
            "rendered": "Trolls World Tour (2020)"
        },
        "content": {
            "rendered": "<p>In the techno kingdom, King Trollex&#8217;s tribe of Techno Trolls are attending a rave, when Hard Rock Trolls led by Queen Barb arrive and use weaponized guitars to destroy everything. Barb demands Trollex surrender and hand over his &#8220;string&#8221;. In Pop Village, two years after making peace with the Bergens, Poppy, still trying to get used to being queen of the Pop Trolls, receives a letter from Barb, inviting her to bring her &#8220;string&#8221; to unite the trolls.<\/p>\n",
            "protected": false
        },
        "excerpt": {
            "rendered": "<p>In the techno kingdom, King Trollex&#8217;s tribe of Techno Trolls are attending a rave, when Hard Rock Trolls led by Queen Barb arrive and use weaponized guitars to destroy everything&#8230;. <a class=\"read-more\" href=\"http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/\">Read More<\/a><\/p>\n",
            "protected": false
        },
        "author": 2,
        "featured_media": 56,
        "comment_status": "open",
        "ping_status": "open",
        "sticky": false,
        "template": "",
        "format": "standard",
        "meta": {
            "_edit_lock": [
                "1739546593:1"
            ],
            "_edit_last": [
                "1"
            ],
            "_thumbnail_id": [
                "56"
            ],
            "publication": [
                "Empower99"
            ],
            "_publication": [
                "field_67a34d2fe986e"
            ],
            "preview": [
                "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/"
            ],
            "_preview": [
                "field_67a34d3be986f"
            ],
            "live_preview": [
                ""
            ],
            "_live_preview": [
                "field_67a34d46e9870"
            ],
            "website": [
                "http:\/\/orbit.k8or.com\/"
            ],
            "_website": [
                "field_67a34d4ee9871"
            ],
            "website-main-article": [
                "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/"
            ],
            "_website-main-article": [
                "field_67a34d52e9872"
            ],
            "plan": [
                "coming soon"
            ],
            "_plan": [
                "field_67a34d5ae9873"
            ],
            "recommender": [
                "None"
            ],
            "_recommender": [
                "field_67a34d69e9874"
            ],
            "recommender_url": [
                ""
            ],
            "_recommender_url": [
                "field_67a34d6ce9875"
            ],
            "preview_button_text": [
                "Reveal the Data"
            ],
            "_preview_button_text": [
                "field_67a34d74e9876"
            ],
            "start_date": [
                "20250207"
            ],
            "_start_date": [
                "field_67a34d82e9877"
            ],
            "version": [
                "v0-0-01"
            ],
            "_version": [
                "field_67a37259b0c3a"
            ],
            "scanned": [
                "No"
            ],
            "_scanned": [
                "field_67a372a802406"
            ],
            "part": [
                "7893"
            ],
            "_part": [
                "field_67a3735718fc6"
            ],
            "image_status": [
                "Transferred"
            ],
            "_image_status": [
                "field_67a37d949ec30"
            ],
            "imageuid": [
                "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5"
            ],
            "_imageuid": [
                "field_67aa0c4581976"
            ],
            "environment": [
                "Testing"
            ],
            "_environment": [
                "field_67aa0c5481977"
            ]
        },
        "categories": [
            3
        ],
        "tags": [
            5
        ],
        "variable": [
            12
        ],
        "part": [],
        "class_list": [
            "post-53",
            "post",
            "type-post",
            "status-publish",
            "format-standard",
            "has-post-thumbnail",
            "hentry",
            "category-movie",
            "tag-development",
            "variable-part-number"
        ],
        "acf": {
            "publication": "Empower99",
            "preview": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/",
            "live_preview": "",
            "website": "http:\/\/orbit.k8or.com\/",
            "website-main-article": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/",
            "plan": "coming soon",
            "recommender": "None",
            "recommender_url": "",
            "preview_button_text": "Reveal the Data",
            "start_date": "20250207",
            "version": "v0-0-01",
            "scanned": "No",
            "part": "7893",
            "image_status": "Transferred",
            "imageuid": "Het5SfSA6NJdRVdhTUhwev9A528QH4gwdJQMSA857PStLeyRnpGffDpDMJq5",
            "environment": "Testing"
        },
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/53",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/post"
                }
            ],
            "author": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/2"
                }
            ],
            "replies": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=53"
                }
            ],
            "version-history": [
                {
                    "count": 16,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/53\/revisions"
                }
            ],
            "predecessor-version": [
                {
                    "id": 80,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts\/53\/revisions\/80"
                }
            ],
            "wp:featuredmedia": [
                {
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/56"
                }
            ],
            "wp:attachment": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media?parent=53"
                }
            ],
            "wp:term": [
                {
                    "taxonomy": "category",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories?post=53"
                },
                {
                    "taxonomy": "post_tag",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags?post=53"
                },
                {
                    "taxonomy": "variable",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable?post=53"
                },
                {
                    "taxonomy": "part",
                    "embeddable": true,
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part?post=53"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        },
        "_embedded": {
            "author": [
                {
                    "id": 2,
                    "name": "James Bond",
                    "url": "http:\/\/www.k8or.com",
                    "description": "",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/author\/jamesbond\/",
                    "slug": "jamesbond",
                    "avatar_urls": {
                        "24": "https:\/\/secure.gravatar.com\/avatar\/468350c2f7811952df086715ea95e3c9?s=24&d=mm&r=g",
                        "48": "https:\/\/secure.gravatar.com\/avatar\/468350c2f7811952df086715ea95e3c9?s=48&d=mm&r=g",
                        "96": "https:\/\/secure.gravatar.com\/avatar\/468350c2f7811952df086715ea95e3c9?s=96&d=mm&r=g"
                    },
                    "acf": [],
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/2",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users"
                            }
                        ]
                    }
                }
            ],
            "wp:featuredmedia": [
                {
                    "id": 56,
                    "date": "2025-02-07T11:53:51",
                    "slug": "trolls-logo1-1024x1024",
                    "type": "attachment",
                    "link": "http:\/\/orbit.k8or.com\/viewport\/trolls-world-tour-2020\/trolls-logo1-1024x1024\/",
                    "title": {
                        "rendered": "TROLLS-LOGO1-1024&#215;1024"
                    },
                    "author": 2,
                    "featured_media": 0,
                    "acf": [],
                    "caption": {
                        "rendered": ""
                    },
                    "alt_text": "",
                    "media_type": "image",
                    "mime_type": "image\/webp",
                    "media_details": {
                        "width": 1024,
                        "height": 1024,
                        "file": "2025\/02\/TROLLS-LOGO1-1024x1024-1.webp",
                        "filesize": 38734,
                        "sizes": {
                            "medium": {
                                "file": "TROLLS-LOGO1-1024x1024-1-300x300.webp",
                                "width": 300,
                                "height": 300,
                                "filesize": 6562,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1-300x300.webp"
                            },
                            "thumbnail": {
                                "file": "TROLLS-LOGO1-1024x1024-1-150x150.webp",
                                "width": 150,
                                "height": 150,
                                "filesize": 2944,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1-150x150.webp"
                            },
                            "medium_large": {
                                "file": "TROLLS-LOGO1-1024x1024-1-768x768.webp",
                                "width": 768,
                                "height": 768,
                                "filesize": 20020,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1-768x768.webp"
                            },
                            "full": {
                                "file": "TROLLS-LOGO1-1024x1024-1.webp",
                                "width": 1024,
                                "height": 1024,
                                "mime_type": "image\/webp",
                                "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1.webp"
                            }
                        },
                        "image_meta": {
                            "aperture": "0",
                            "credit": "",
                            "camera": "",
                            "caption": "",
                            "created_timestamp": "0",
                            "copyright": "",
                            "focal_length": "0",
                            "iso": "0",
                            "shutter_speed": "0",
                            "title": "",
                            "orientation": "0",
                            "keywords": []
                        }
                    },
                    "source_url": "http:\/\/orbit.k8or.com\/viewport\/wp-content\/uploads\/2025\/02\/TROLLS-LOGO1-1024x1024-1.webp",
                    "_links": {
                        "self": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media\/56",
                                "targetHints": {
                                    "allow": [
                                        "GET"
                                    ]
                                }
                            }
                        ],
                        "collection": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/media"
                            }
                        ],
                        "about": [
                            {
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/types\/attachment"
                            }
                        ],
                        "author": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/users\/2"
                            }
                        ],
                        "replies": [
                            {
                                "embeddable": true,
                                "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/comments?post=56"
                            }
                        ]
                    }
                }
            ],
            "wp:term": [
                [
                    {
                        "id": 3,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/movie\/",
                        "name": "Movie",
                        "slug": "movie",
                        "taxonomy": "category",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/3",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=3"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 5,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/tag\/development\/",
                        "name": "Development",
                        "slug": "development",
                        "taxonomy": "post_tag",
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags\/5",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/post_tag"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?tags=5"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                [
                    {
                        "id": 12,
                        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/part-number\/",
                        "name": "Part number",
                        "slug": "part-number",
                        "taxonomy": "variable",
                        "acf": [],
                        "_links": {
                            "self": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/12",
                                    "targetHints": {
                                        "allow": [
                                            "GET"
                                        ]
                                    }
                                }
                            ],
                            "collection": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                                }
                            ],
                            "about": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                                }
                            ],
                            "wp:post_type": [
                                {
                                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=12"
                                }
                            ],
                            "curies": [
                                {
                                    "name": "wp",
                                    "href": "https:\/\/api.w.org\/{rel}",
                                    "templated": true
                                }
                            ]
                        }
                    }
                ],
                []
            ]
        }
    }
]
```

### 4. c8fms06-c8bms18-e20: The **`Portal Upload Board` Frontend Microservice** `c8fms06` communicates with the **`Portal Search Logic` Backend Microservice** `c8bms18` to retrieve categories from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/categories?per_page=100&hierarchical=0&parent=0&hide_empty=true`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 3,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/movie\/",
        "name": "Movie",
        "slug": "movie",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/3",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=3"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 4,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/music\/",
        "name": "Music",
        "slug": "music",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/4",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=4"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 5. c8bms18-c32bre03-e25: The **`Portal Search Logic` Backend Microservice** `c8bms18` retrieves categories from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/categories?per_page=100&hierarchical=0&parent=0&hide_empty=true`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 3,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/movie\/",
        "name": "Movie",
        "slug": "movie",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/3",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=3"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 4,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/music\/",
        "name": "Music",
        "slug": "music",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/4",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=4"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 6. c8fms06-c8bms18-e30: The **`Portal Upload Board` Frontend Microservice** `c8fms06` communicates with the **`Portal Search Logic` Backend Microservice** `c8bms18` to retrieve categories from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/categories?per_page=100&hierarchical=0&hide_empty=true`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 3,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/movie\/",
        "name": "Movie",
        "slug": "movie",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/3",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=3"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 4,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/music\/",
        "name": "Music",
        "slug": "music",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/4",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=4"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 7. c8bms18-c32bre03-e35: The **`Portal Search Logic` Backend Microservice** `c8bms18` retrieve categories from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/categories?per_page=100&hierarchical=0&hide_empty=true`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 3,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/movie\/",
        "name": "Movie",
        "slug": "movie",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/3",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=3"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 4,
        "count": 1,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/category\/music\/",
        "name": "Music",
        "slug": "music",
        "taxonomy": "category",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories\/4",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/categories"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/category"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?categories=4"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 8. c8fms06-c8bms18-e40: The **`Portal Upload Board` Frontend Microservice** `c8fms06` communicates with the **`Portal Search Logic` Backend Microservice** `c8bms18` to retrieve tags from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/tags?per_page=100`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
---
```json
[
    {
        "id": 5,
        "count": 2,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/tag\/development\/",
        "name": "Development",
        "slug": "development",
        "taxonomy": "post_tag",
        "meta": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags\/5",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/post_tag"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?tags=5"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 9. c8bms18-c32bre03-e45: The **`Portal Search Logic` Backend Microservice** `c8bms18` retrieves tags from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/tags?per_page=100`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
---
```json
[
    {
        "id": 5,
        "count": 2,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/tag\/development\/",
        "name": "Development",
        "slug": "development",
        "taxonomy": "post_tag",
        "meta": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags\/5",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/tags"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/post_tag"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?tags=5"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 10. c8fms06-c8bms18-e50: The **`Portal Upload Board` Frontend Microservice** `c8fms06` communicates with the **`Portal Search Logic` Backend Microservice** `c8bms18` to retrieve variables from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/variable`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 12,
        "count": 2,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/part-number\/",
        "name": "Part number",
        "slug": "part-number",
        "taxonomy": "variable",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/12",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=12"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 13,
        "count": 0,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/tail\/",
        "name": "Tail",
        "slug": "tail",
        "taxonomy": "variable",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/13",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=13"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 11. c8bms18-c32bre03-e55: The **`Portal Search Logic` Backend Microservice** `c8bms18` retrieves variables from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/variable`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 12,
        "count": 2,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/part-number\/",
        "name": "Part number",
        "slug": "part-number",
        "taxonomy": "variable",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/12",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=12"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 13,
        "count": 0,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/variable\/tail\/",
        "name": "Tail",
        "slug": "tail",
        "taxonomy": "variable",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable\/13",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/variable"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/variable"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?variable=13"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 12. c8fms06-c8bms18-e60: The **`Portal Upload Board` Frontend Microservice** `c8fms06` communicates with the **`Portal Search Logic` Backend Microservice** `c8bms18` to retrieve parts from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/part`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 15,
        "count": 0,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/part\/4562\/",
        "name": "4562",
        "slug": "4562",
        "taxonomy": "part",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part\/15",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/part"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?part=15"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 14,
        "count": 0,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/part\/7893\/",
        "name": "7893",
        "slug": "7893",
        "taxonomy": "part",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part\/14",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/part"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?part=14"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 13. c8bms18-c32bre03-e65: The **`Portal Search Logic` Backend Microservice** `c8bms18` retrieves parts from the MySQL database.
* **Endpoint:** `http://orbit.k8or.com/viewport/wp-json/wp/v2/part`
* **Protocol type:** `https`
* **Request type:** `GET`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** 
```json
[
    {
        "id": 15,
        "count": 0,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/part\/4562\/",
        "name": "4562",
        "slug": "4562",
        "taxonomy": "part",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part\/15",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/part"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?part=15"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    },
    {
        "id": 14,
        "count": 0,
        "description": "",
        "link": "http:\/\/orbit.k8or.com\/viewport\/part\/7893\/",
        "name": "7893",
        "slug": "7893",
        "taxonomy": "part",
        "parent": 0,
        "meta": [],
        "acf": [],
        "_links": {
            "self": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part\/14",
                    "targetHints": {
                        "allow": [
                            "GET"
                        ]
                    }
                }
            ],
            "collection": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/part"
                }
            ],
            "about": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/taxonomies\/part"
                }
            ],
            "wp:post_type": [
                {
                    "href": "http:\/\/orbit.k8or.com\/viewport\/wp-json\/wp\/v2\/posts?part=14"
                }
            ],
            "curies": [
                {
                    "name": "wp",
                    "href": "https:\/\/api.w.org\/{rel}",
                    "templated": true
                }
            ]
        }
    }
]
```

### 14. **`c8bms18-c20bre24-EEE`:** The `Portal Search Logic` Backend Microservice writes error logs to the `user-error-log-1624-tbl-k8d-aws` DynamoDB table.

#### Each DynamoDB function must record `createdTimestampUTC` and `createdTimestampUnix` timestamps.

* **DynamoDB Table Schema:** 
    * **Partition key:** `username` (String)
    * **Sort key:** `errorType` (String)
* **Endpoint:** `https://orbit.k8or.com/upload/GetUserDataViewPort.php`
```
upload/
├── src/
│   ├── routes/
│   │   └── ErrorLog.php
│   ├── functions/
│   └── config.php 
├── composer.json
└── vendor/
```
* **Protocol type:** `https`
* **Request type:** `POST`
* **Synchronicity:** `async` 
* **Request payload:** `none`
* **Response payload:** `none`