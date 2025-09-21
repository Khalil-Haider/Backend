# Ultimate FastAPI Master Reference Table

## 1. CORE FOUNDATION

| Concept | Description | Code Example | Use Cases |
|---------|-------------|--------------|-----------|
| **FastAPI Instance** | Main application object | `app = FastAPI()` | Entry point for all routes and configuration |
| **Path Operation** | Function that handles HTTP requests | `@app.get("/items")` | Define API endpoints |
| **Path Operation Decorator** | Decorator that defines HTTP method + path | `@app.get()`, `@app.post()` | Route definition |
| **Path Operation Function** | The actual function that processes requests | `async def read_items():` | Business logic implementation |
| **ASGI** | Asynchronous Server Gateway Interface | FastAPI is ASGI-compatible | High-performance async web apps |

## 2. HTTP METHODS & OPERATIONS

| Method | CRUD Operation | Description | Status Codes | Example |
|--------|----------------|-------------|--------------|---------|
| **GET** | Read | Retrieve data, idempotent | 200, 404 | `@app.get("/items/{item_id}")` |
| **POST** | Create | Submit new data, not idempotent | 201, 400, 422 | `@app.post("/items/")` |
| **PUT** | Update (Full) | Replace entire resource | 200, 204, 404 | `@app.put("/items/{item_id}")` |
| **PATCH** | Update (Partial) | Modify part of resource | 200, 204, 404 | `@app.patch("/items/{item_id}")` |
| **DELETE** | Delete | Remove resource | 204, 404 | `@app.delete("/items/{item_id}")` |
| **HEAD** | Metadata | Get headers without body | 200, 404 | `@app.head("/items/{item_id}")` |
| **OPTIONS** | Options | Get allowed methods | 200 | `@app.options("/items/")` |
| **TRACE** | Trace | Diagnostic loopback | 200 | `@app.trace("/items/")` |

## 3. PATH OPERATIONS & ROUTING

| Concept | Description | Example | Notes |
|---------|-------------|---------|-------|
| **Static Paths** | Fixed URL segments | `@app.get("/users/")` | Exact match required |
| **Path Parameters** | Dynamic URL segments | `@app.get("/users/{user_id}")` | Captured as function parameters |
| **Path Parameter Types** | Type conversion for path params | `@app.get("/items/{item_id:int}")` | Built-in converters |
| **Multiple Path Parameters** | Multiple dynamic segments | `@app.get("/users/{user_id}/items/{item_id}")` | Order matters in function signature |
| **Path Parameter Validation** | Constraints on path params | `Path(..., gt=0, le=1000)` | Using Path() function |
| **Wildcard Paths** | Catch-all path segments | `@app.get("/files/{file_path:path}")` | Captures remaining path |

## 4. QUERY PARAMETERS

| Concept | Description | Example | Default Behavior |
|---------|-------------|---------|------------------|
| **Basic Query Parameters** | URL parameters after ? | `def read_items(skip: int = 0):` | Optional with default values |
| **Required Query Parameters** | Mandatory query params | `def read_items(q: str):` | No default value = required |
| **Multiple Query Parameters** | Several query params | `def read_items(q: str, skip: int = 0, limit: int = 10):` | Combined in any order |
| **Query Parameter Types** | Type conversion | `skip: int`, `active: bool` | Automatic validation |
| **Query Parameter Validation** | Constraints on query params | `Query(..., min_length=3, max_length=50)` | Using Query() function |
| **Optional Query Parameters** | Nullable parameters | `q: Optional[str] = None` | Union types |
| **List Query Parameters** | Multiple values for same param | `tags: List[str] = Query([])` | ?tags=foo&tags=bar |

## 5. REQUEST BODY & DATA MODELS

| Concept | Description | Example | Purpose |
|---------|-------------|---------|---------|
| **Pydantic Models** | Data validation and serialization | `class Item(BaseModel): name: str` | Type-safe data structures |
| **Request Body** | JSON payload in requests | `def create_item(item: Item):` | POST/PUT/PATCH data |
| **Nested Models** | Models within models | `class User(BaseModel): items: List[Item]` | Complex data structures |
| **Model Validation** | Automatic data validation | Field validators, custom validators | Data integrity |
| **Model Serialization** | Convert models to JSON | Automatic with response_model | API responses |
| **Field Validation** | Individual field constraints | `Field(..., min_length=1, max_length=100)` | Granular validation |
| **Custom Validators** | Custom validation logic | `@validator('email')` | Business rules |
| **Model Config** | Model behavior configuration | `class Config: orm_mode = True` | ORM integration |

## 6. STATUS CODES

| Category | Range | Common Codes | Description | FastAPI Usage |
|----------|-------|--------------|-------------|---------------|
| **1xx Informational** | 100-199 | 100, 101 | Request processing | Rarely used |
| **2xx Success** | 200-299 | 200, 201, 204 | Successful requests | Default responses |
| **3xx Redirection** | 300-399 | 301, 302, 304 | Further action needed | Redirect responses |
| **4xx Client Error** | 400-499 | 400, 401, 403, 404, 422 | Client-side errors | Validation, auth errors |
| **5xx Server Error** | 500-599 | 500, 502, 503 | Server-side errors | Internal errors |

### Detailed Status Codes

| Code | Name | Description | FastAPI Context |
|------|------|-------------|-----------------|
| **200** | OK | Standard success response | Default for GET |
| **201** | Created | Resource created successfully | POST operations |
| **204** | No Content | Success with no response body | DELETE operations |
| **400** | Bad Request | Invalid request syntax | Validation errors |
| **401** | Unauthorized | Authentication required | Auth failures |
| **403** | Forbidden | Access denied | Authorization failures |
| **404** | Not Found | Resource not found | Missing resources |
| **422** | Unprocessable Entity | Validation errors | Pydantic validation |
| **500** | Internal Server Error | Server-side errors | Unhandled exceptions |

## 7. RESPONSE HANDLING

| Concept | Description | Example | Use Case |
|---------|-------------|---------|----------|
| **Response Model** | Define response structure | `response_model=List[Item]` | Type-safe responses |
| **Response Model Exclusion** | Hide sensitive fields | `response_model_exclude={"password"}` | Security |
| **Response Model Inclusion** | Show specific fields only | `response_model_include={"name", "email"}` | Data filtering |
| **Custom Response** | Manual response creation | `return JSONResponse(content={})` | Custom headers/status |
| **Response Classes** | Different response types | `HTMLResponse`, `PlainTextResponse` | Content type control |
| **File Responses** | Serve files | `FileResponse("file.pdf")` | File downloads |
| **Streaming Responses** | Stream large data | `StreamingResponse(generate_data())` | Large datasets |
| **Response Headers** | Custom response headers | `Response(headers={"X-Custom": "value"})` | Metadata |

## 8. VALIDATION & SERIALIZATION

| Concept | Description | Implementation | Benefits |
|---------|-------------|----------------|----------|
| **Automatic Validation** | Request data validation | Pydantic models | Type safety |
| **Field Validators** | Custom field validation | `@validator('field_name')` | Business rules |
| **Root Validators** | Validate entire model | `@root_validator` | Cross-field validation |
| **Type Hints** | Python type annotations | `name: str`, `age: int` | IDE support, validation |
| **Union Types** | Multiple possible types | `Union[str, int]` | Flexible inputs |
| **Optional Fields** | Nullable fields | `Optional[str]` | Not required |
| **Default Values** | Field defaults | `name: str = "default"` | Fallback values |
| **Alias Fields** | Alternative field names | `Field(alias="userName")` | API compatibility |

## 9. AUTHENTICATION & AUTHORIZATION

| Concept | Description | Implementation | Use Case |
|---------|-------------|----------------|----------|
| **OAuth2** | OAuth 2.0 implementation | `OAuth2PasswordBearer` | Token-based auth |
| **JWT Tokens** | JSON Web Tokens | `python-jose[cryptography]` | Stateless auth |
| **Password Hashing** | Secure password storage | `passlib[bcrypt]` | Password security |
| **Scopes** | Permission granularity | `scopes={"read": "Read access"}` | Fine-grained permissions |
| **Dependencies** | Reusable auth logic | `Depends(get_current_user)` | Auth injection |
| **Security Schemes** | OpenAPI security docs | Various security definitions | API documentation |
| **API Keys** | Simple authentication | `APIKeyHeader`, `APIKeyQuery` | Basic auth |
| **HTTP Basic Auth** | Username/password auth | `HTTPBasic` | Simple auth |

## 10. DEPENDENCIES

| Concept | Description | Example | Purpose |
|---------|-------------|---------|---------|
| **Dependency Injection** | Provide dependencies to functions | `Depends(get_db)` | Separation of concerns |
| **Function Dependencies** | Dependencies as functions | `def get_db(): ...` | Resource management |
| **Class Dependencies** | Dependencies as classes | `class CommonDependency: ...` | Stateful dependencies |
| **Sub-dependencies** | Dependencies with dependencies | `def get_user(db=Depends(get_db)):` | Dependency chains |
| **Dependency Override** | Override for testing | `app.dependency_overrides[get_db] = override_get_db` | Testing |
| **Global Dependencies** | App-level dependencies | `dependencies=[Depends(verify_token)]` | Cross-cutting concerns |
| **Path-level Dependencies** | Router-level dependencies | Router dependencies | Route groups |
| **Yield Dependencies** | Context manager dependencies | `yield db` | Resource cleanup |

## 11. BACKGROUND TASKS

| Concept | Description | Example | Use Case |
|---------|-------------|---------|----------|
| **Background Tasks** | Non-blocking task execution | `BackgroundTasks.add_task()` | Email sending |
| **Task Functions** | Functions run in background | `def send_notification():` | Async operations |
| **Task Parameters** | Pass data to background tasks | `add_task(func, param1, param2)` | Parameterized tasks |
| **Multiple Tasks** | Queue multiple tasks | Multiple `add_task()` calls | Batch operations |

## 12. MIDDLEWARE

| Type | Description | Implementation | Purpose |
|------|-------------|----------------|---------|
| **Custom Middleware** | Custom processing logic | `@app.middleware("http")` | Request/response processing |
| **CORS Middleware** | Cross-Origin Resource Sharing | `CORSMiddleware` | Browser security |
| **Trusted Host Middleware** | Host validation | `TrustedHostMiddleware` | Security |
| **GZIP Middleware** | Response compression | `GZipMiddleware` | Performance |
| **HTTPS Redirect** | Force HTTPS | `HTTPSRedirectMiddleware` | Security |
| **Session Middleware** | Session management | Third-party middleware | Stateful sessions |

## 13. ERROR HANDLING

| Concept | Description | Example | Purpose |
|---------|-------------|---------|---------|
| **HTTPException** | HTTP error responses | `raise HTTPException(404, "Not found")` | Standard HTTP errors |
| **Custom Exception Handlers** | Handle specific exceptions | `@app.exception_handler(ValueError)` | Custom error responses |
| **Validation Errors** | Pydantic validation errors | Automatic handling | Input validation |
| **Request Validation Error** | Request parsing errors | Built-in handling | Malformed requests |
| **Custom Error Responses** | Tailored error messages | Custom exception classes | User-friendly errors |

## 14. CONFIGURATION & SETTINGS

| Concept | Description | Implementation | Use Case |
|---------|-------------|----------------|----------|
| **Settings Management** | Application configuration | `BaseSettings` | Environment config |
| **Environment Variables** | Config from env vars | `.env` files | Deployment flexibility |
| **Config Validation** | Validate configuration | Pydantic settings | Config integrity |
| **Multiple Environments** | Different configs per env | Environment-specific settings | Dev/staging/prod |

## 15. TESTING

| Concept | Description | Tool/Library | Purpose |
|---------|-------------|--------------|---------|
| **Test Client** | Testing FastAPI apps | `TestClient` | Integration testing |
| **Async Testing** | Test async endpoints | `pytest-asyncio` | Async test support |
| **Database Testing** | Test with databases | Test database setup | Data layer testing |
| **Dependency Override** | Mock dependencies | `app.dependency_overrides` | Isolated testing |
| **Fixture Management** | Test data setup | `pytest` fixtures | Test data |

## 16. DATABASE INTEGRATION

| Concept | Description | Library | Use Case |
|---------|-------------|---------|----------|
| **SQLAlchemy Integration** | ORM integration | `SQLAlchemy` | Relational databases |
| **Async Databases** | Async database operations | `databases`, `asyncpg` | High performance |
| **Database Sessions** | Connection management | Session dependencies | Resource management |
| **Migration Management** | Database schema changes | `Alembic` | Schema evolution |
| **Connection Pooling** | Optimize database connections | SQLAlchemy pools | Performance |

## 17. WEBSOCKETS

| Concept | Description | Implementation | Use Case |
|---------|-------------|----------------|----------|
| **WebSocket Endpoints** | Real-time communication | `@app.websocket("/ws")` | Live updates |
| **WebSocket Accept** | Accept connections | `await websocket.accept()` | Connection establishment |
| **Send/Receive Data** | Bidirectional communication | `send_text()`, `receive_text()` | Real-time data |
| **Connection Management** | Handle connections/disconnections | Connection lifecycle | Resource cleanup |
| **WebSocket Dependencies** | Inject dependencies | `Depends()` in WebSocket | Shared logic |

## 18. ADVANCED ROUTING

| Concept | Description | Example | Use Case |
|---------|-------------|---------|----------|
| **APIRouter** | Modular routing | `router = APIRouter()` | Code organization |
| **Route Inclusion** | Include routers | `app.include_router(router)` | Modular apps |
| **Route Prefixes** | Common path prefixes | `prefix="/api/v1"` | API versioning |
| **Route Tags** | Group operations | `tags=["items"]` | Documentation |
| **Route Dependencies** | Router-level dependencies | `dependencies=[Depends(auth)]` | Shared logic |
| **Sub-applications** | Mount sub-apps | `app.mount("/sub", sub_app)` | App composition |

## 19. OPENAPI & DOCUMENTATION

| Concept | Description | Configuration | Purpose |
|---------|-------------|---------------|---------|
| **Automatic Documentation** | Generated API docs | Built-in | Developer experience |
| **Swagger UI** | Interactive documentation | `/docs` endpoint | API exploration |
| **ReDoc** | Alternative documentation | `/redoc` endpoint | Clean documentation |
| **OpenAPI Schema** | API specification | `/openapi.json` | Standards compliance |
| **Custom Documentation** | Tailored docs | Metadata configuration | Branding |
| **Operation Descriptions** | Endpoint descriptions | Docstrings, descriptions | Documentation quality |

## 20. PERFORMANCE & OPTIMIZATION

| Concept | Description | Implementation | Benefit |
|---------|-------------|----------------|---------|
| **Async/Await** | Asynchronous programming | `async def` functions | Concurrent request handling |
| **Database Connection Pooling** | Reuse database connections | SQLAlchemy pools | Reduced connection overhead |
| **Caching** | Store computed results | Redis, in-memory | Faster responses |
| **Response Compression** | Compress HTTP responses | GZip middleware | Reduced bandwidth |
| **Pagination** | Limit response size | Query parameters | Manageable data sets |
| **Response Models** | Optimize serialization | Exclude unnecessary fields | Smaller payloads |

## 21. DEPLOYMENT & PRODUCTION

| Concept | Description | Tool/Method | Purpose |
|---------|-------------|-------------|---------|
| **ASGI Servers** | Production servers | `uvicorn`, `gunicorn` | Production serving |
| **Process Managers** | Multi-process serving | `gunicorn` with workers | Scalability |
| **Docker Deployment** | Containerization | Docker images | Consistent deployment |
| **Environment Configuration** | Production settings | Environment variables | Configuration management |
| **Health Checks** | Application monitoring | Health endpoints | Service monitoring |
| **Logging** | Application logging | Python logging | Debugging, monitoring |

## 22. SECURITY

| Concept | Description | Implementation | Protection Against |
|---------|-------------|----------------|-------------------|
| **HTTPS Enforcement** | Force encrypted connections | HTTPS redirect middleware | Man-in-the-middle attacks |
| **CORS Configuration** | Cross-origin request control | CORS middleware | XSS attacks |
| **Input Validation** | Validate all inputs | Pydantic models | Injection attacks |
| **Authentication** | Verify user identity | OAuth2, JWT | Unauthorized access |
| **Authorization** | Control resource access | Scopes, permissions | Privilege escalation |
| **Rate Limiting** | Limit request rates | Custom middleware | DoS attacks |
| **SQL Injection Prevention** | Safe database queries | ORM usage | SQL injection |

## 23. ADVANCED FEATURES

| Concept | Description | Use Case | Implementation |
|---------|-------------|----------|----------------|
| **Custom Response Classes** | Specialized responses | File downloads, templates | Inherit from Response |
| **Request/Response Lifecycle** | Hooks into request processing | Logging, monitoring | Middleware |
| **Custom Serializers** | Non-JSON responses | XML, CSV responses | Custom response classes |
| **GraphQL Integration** | GraphQL support | Complex queries | Strawberry, Graphene |
| **Server-Sent Events** | Real-time updates | Live feeds | StreamingResponse |
| **File Uploads** | Handle file uploads | Form data, multipart | UploadFile |
| **Template Rendering** | HTML responses | Web interfaces | Jinja2 templates |

## 24. INTEGRATION PATTERNS

| Pattern | Description | Use Case | Benefits |
|---------|-------------|----------|----------|
| **Repository Pattern** | Data access abstraction | Database operations | Testability, flexibility |
| **Service Layer** | Business logic separation | Complex operations | Code organization |
| **Factory Pattern** | Object creation | Database connections | Dependency management |
| **Singleton Pattern** | Single instance objects | Configuration, caching | Resource sharing |
| **Observer Pattern** | Event-driven architecture | Notifications | Loose coupling |

## 25. MONITORING & OBSERVABILITY

| Concept | Description | Tool/Method | Purpose |
|---------|-------------|-------------|---------|
| **Metrics Collection** | Application metrics | Prometheus, custom | Performance monitoring |
| **Distributed Tracing** | Request tracing | Jaeger, Zipkin | Debug distributed systems |
| **Health Checks** | Service health monitoring | Custom endpoints | Service monitoring |
| **Logging** | Application logging | Structured logging | Debugging, auditing |
| **Error Tracking** | Error monitoring | Sentry, custom | Issue identification |

This master reference covers every FastAPI concept from basic routing to advanced production deployment patterns. Each section builds upon previous concepts while maintaining clear categorization for easy reference and learning progression.