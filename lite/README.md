## README for Dify Compose Lite

Welcome to the lite version for Dify compose deployment

### What's Changed

- Split the Dify data storage path to the .env file for unified configuration.
- Add network proxy settings.
- Removed vector database support: QDrant, Milvus, MyScale, CouchBase, PGVector/PGVector-RS, TiDB, Chroma, Oracle, Relyt, OpenSearch, ElasticSearch, OceanBase, Opengauss.
- Removed middleware support.
- Adjust API Worker from 1 to 3.
- Adjust default ETL type to Unstructured.
- **\[Security\]** Team member invitation validity period reduced from 72 hours to 8 hours.
- **\[Security\]** Default Web service port changed from 80/443 to 8080/8443.
- Set the default PIP mirror source for Plugin Daemon to Aliyun.

About Network Proxy:

In current version(v1.1.0), enable proxy for dify api will cause serious network problem including unable to use embedding/reranker, so network proxy setting is NOT apply to anything by default.

### How to Deploy Dify with `docker-compose.yaml`

1. **Prerequisites**: Ensure Docker and Docker Compose are installed on your system.
2. **Environment Setup**:
    - Navigate to the `docker` directory.
    - Copy the `.env.example` file to a new file named `.env` by running `cp .env.example .env`.
    - Customize the `.env` file as needed. Refer to the `.env.example` file for detailed configuration options.
3. **Running the Services**:
    - Execute `docker compose up` from the `docker` directory to start the services.
    - To specify a vector database, set the `VECTOR_STORE` variable in your `.env` file to your desired vector database service, such as `milvus`, `weaviate`, or `opensearch`.
4. **SSL Certificate Setup**:
    - Refer `docker/certbot/README.md` to set up SSL certificates using Certbot.
5. **OpenTelemetry Collector Setup**:
   - Change `ENABLE_OTEL` to `true` in `.env`.
   - Configure `OTLP_BASE_ENDPOINT` properly.

### Migration for Existing Users

For users migrating from the `docker-legacy` setup:

1. **Review Changes**: Familiarize yourself with the new `.env` configuration and Docker Compose setup.
2. **Transfer Customizations**:
    - If you have customized configurations such as `docker-compose.yaml`, `ssrf_proxy/squid.conf`, or `nginx/conf.d/default.conf`, you will need to reflect these changes in the `.env` file you create.
3. **Data Migration**:
    - Ensure that data from services like databases and caches is backed up and migrated appropriately to the new structure if necessary.

### Overview of `.env`

#### Key Modules and Customization

- **Vector Database Services**: Depending on the type of vector database used (`VECTOR_STORE`), users can set specific endpoints, ports, and authentication details.
- **Storage Services**: Depending on the storage type (`STORAGE_TYPE`), users can configure specific settings for S3, Azure Blob, Google Storage, etc.
- **API and Web Services**: Users can define URLs and other settings that affect how the API and web frontend operate.

#### Other notable variables

The `.env.example` file provided in the Docker setup is extensive and covers a wide range of configuration options. It is structured into several sections, each pertaining to different aspects of the application and its services. Here are some of the key sections and variables:

1. **Common Variables**:
    - `CONSOLE_API_URL`, `SERVICE_API_URL`: URLs for different API services.
    - `APP_WEB_URL`: Frontend application URL.
    - `FILES_URL`: Base URL for file downloads and previews.

2. **Server Configuration**:
    - `LOG_LEVEL`, `DEBUG`, `FLASK_DEBUG`: Logging and debug settings.
    - `SECRET_KEY`: A key for encrypting session cookies and other sensitive data.

3. **Database Configuration**:
    - `DB_USERNAME`, `DB_PASSWORD`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`: PostgreSQL database credentials and connection details.

4. **Redis Configuration**:
    - `REDIS_HOST`, `REDIS_PORT`, `REDIS_PASSWORD`: Redis server connection settings.

5. **Celery Configuration**:
    - `CELERY_BROKER_URL`: Configuration for Celery message broker.

6. **Storage Configuration**:
    - `STORAGE_TYPE`, `S3_BUCKET_NAME`, `AZURE_BLOB_ACCOUNT_NAME`: Settings for file storage options like local, S3, Azure Blob, etc.

7. **Vector Database Configuration**:
    - `VECTOR_STORE`: Type of vector database (e.g., `weaviate`, `milvus`).
    - Specific settings for each vector store like `WEAVIATE_ENDPOINT`, `MILVUS_URI`.

8. **CORS Configuration**:
    - `WEB_API_CORS_ALLOW_ORIGINS`, `CONSOLE_CORS_ALLOW_ORIGINS`: Settings for cross-origin resource sharing.

9. **OpenTelemetry Configuration**:
    - `ENABLE_OTEL`: Enable OpenTelemetry collector in api.
    - `OTLP_BASE_ENDPOINT`: Endpoint for your OTLP exporter.
  
10. **Other Service-Specific Environment Variables**:
    - Each service like `nginx`, `redis`, `db`, and vector databases have specific environment variables that are directly referenced in the `docker-compose.yaml`.

### Additional Information

- **Continuous Improvement Phase**: We are actively seeking feedback from the community to refine and enhance the deployment process. As more users adopt this new method, we will continue to make improvements based on your experiences and suggestions.
- **Support**: For detailed configuration options and environment variable settings, refer to the `.env.example` file and the Docker Compose configuration files in the `docker` directory.

This README aims to guide you through the deployment process using the new Docker Compose setup. For any issues or further assistance, please refer to the official documentation or contact support.
