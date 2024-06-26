# HDML Documentation

## What is HDML?

HDML, or Hyperdata Markup Language, is a markup language designed for describing hierarchical data structures in a web-based environment. It provides a set of custom HTML components to represent and organize various data models. HDML is particularly useful for creating structured and interactive representations of complex data relationships on the web.

## Distributed Document Concept:

HDML supports the concept of Distributed Documents, allowing you to split one document into several independent documents for security or organizational reasons. This approach enables you to reference and include external documents where necessary.

### Key Points

1. **Separation of Concerns:**
   - Distributed Documents facilitate the separation of different aspects of your data model for better organization and security.

2. **External Inclusion:**
   - Use the concept of Distributed Documents to split your model into modular components.

3. **HDIO Server Processing:**
   - The merging of included documents and processing occur on the HDIO (Hyperdata Input-Output) server.

4. **SQL Query Generation:**
   - Included documents contribute to the generation of an SQL query, executed on the server.

## HDIO Server

The HDIO (Hyperdata Input-Output) server is a critical component in the HDML (Hyperdata Markup Language) ecosystem. It plays a central role in the processing, execution, and management of HDML documents, especially in the context of Distributed Documents and the `hdml-include` component.

### Key Functions

1. **Document Merging and SQL Query Execution:**
   - The HDIO server is responsible for merging included documents specified by the `<hdml-include>` component in the main document.
   - It generates SQL queries based on the processed HDML document structure and executes them against relevant data sources.

2. **Load Balancing:**
   - HDIO ensures balanced distribution of queries to prevent data sources from being overloaded, particularly in scenarios involving analytical queries.

3. **Security and Access Control:**
   - The HDIO server provides authentication mechanisms to secure document processing and SQL query execution.
   - It facilitates the use of environment variables for managing secrets and sensitive information.

4. **Environment Variable Support:**
   - HDIO allows the specification of environment variables that can be utilized to securely store and manage confidential information, such as database credentials.

5. **Result Delivery in Apache Arrow Format:**
   - HDIO delivers query results in the Apache Arrow format, providing a columnar in-memory data representation.
   - Apache Arrow offers benefits such as efficient data transfer, improved performance, and compatibility across different programming languages.

6. **Execution Optimization:**
   - HDIO optimizes the execution of SQL queries, ensuring efficient processing and minimizing the load on data sources.

### Workflow

1. **Document Ingestion:**
   - HDML documents, including hidden and main documents, are ingested by the HDIO server.

2. **Merging and Processing:**
   - The server merges included documents specified by `<hdml-include>`, creating a cohesive structure for further processing.

3. **SQL Query Generation and Execution:**
   - Based on the merged document structure, the HDIO server generates SQL queries and executes them against the relevant database or system.

4. **Load Balancing and Optimization:**
   - HDIO optimizes query execution and ensures even distribution of queries to prevent overloading data sources.

5. **Result Delivery to Client in Apache Arrow Format:**
   - Only the result of the SQL query, formatted in Apache Arrow, is sent to the client. This format offers advantages in terms of data transfer efficiency, improved analytics, and cross-language compatibility.

The HDIO server significantly enhances the functionality, security, and efficiency of the HDML language, particularly in distributed and data-intensive scenarios.



## Environment Variables in HDML

HDML (Hyperdata Markup Language) supports the use of environment variables within attributes, allowing for dynamic and secure configuration. This feature enables the referencing of environment variables directly in HDML attributes, enhancing flexibility and facilitating the management of sensitive information, such as credentials.

### Syntax

To include an environment variable in an HDML attribute, use the following syntax:

```html
<hdml-element
   attribute="${ENV_VARIABLE_NAME}">
</hdml-element>
```

Replace **`ENV_VARIABLE_NAME`** with the name of the desired environment variable.


### Example:

Consider the following example where an environment variable is used within the `host` attribute of an `<hdml-connection>` element:

```html
<hdml-connection
   name="MyDatabase"
   type="postgres"
   host="${DB_HOST}"
   user="user"
   password="password">
</hdml-connection>
```

In this case, the value of the **`host`** attribute is dynamically set to the value of the **`DB_HOST`** environment variable.

### Benefits of Using Environment Variables in HDML

Using environment variables in HDML attributes provides several advantages that enhance flexibility, security, and adaptability in web application development:

1. **Dynamic Configuration:**
   - HDML attributes can be dynamically configured based on environment variables, allowing for easy adaptation to different environments without modifying the HDML document. This flexibility is especially beneficial during development, testing, and production stages.

2. **Secure Credential Management:**
   - Sensitive information, such as database credentials, can be securely managed by storing them as environment variables. By referencing these variables in HDML attributes, the actual credentials are kept external to the HDML document, reducing the risk of exposing sensitive data.

3. **Environment-Agnostic Documents:**
   - HDML documents can be designed to be environment-agnostic, meaning they can seamlessly function in various environments with distinct configurations. By leveraging environment variables, you create a more portable and adaptable web application.

### Best Practices for Using Environment Variables in HDML

To ensure effective and secure utilization of environment variables within HDML attributes, consider adopting the following best practices:

1. **Consistent Naming Conventions:**
   - Adopt a consistent and descriptive naming convention for environment variables. This practice enhances clarity and makes it easier for developers to identify and understand the purpose of each variable.

2. **Secure Handling of Sensitive Variables:**
   - Exercise caution when using environment variables that contain sensitive information, such as database credentials. Ensure that these variables are properly protected and not exposed unintentionally.

3. **Documentation and Comments:**
   - Clearly document the use of environment variables within your HDML document. Include comments explaining the purpose of each variable and any dependencies. This documentation enhances the understanding of your configuration model.

By incorporating environment variables in HDML attributes, you can create more versatile, secure, and adaptable configurations for your web applications.

## Hook Functions

Hook functions in HDIO serve as a powerful mechanism for customizing HDML documents on the server side. These functions are exported, named `hook`, and reside in a specific hook folder on the HDIO server. The HDIO server automatically discovers and executes these functions during the document processing pipeline, allowing users to dynamically modify the merged HDML document based on various conditions, including authentication details.

### Overview

- **Exported Function:** The hook function must be exported from its module and named `hook`. The server automatically detects and executes this function.

- **Location:** Place the file containing the `hook` function in the designated hook folder on the HDIO server.

- **Parameters:**
  - `dom` (Type: `Element`): The HTML element representing the HDML document.
  - `scope` (Type: `Object`): The context derived from the HTTP token of the initial request, typically containing authentication details.

- **Return Value:** The hook function must return the modified or unmodified HTML element (`dom`).

### Example Hook Function

Create a file named `hook.js` with the following content and place it in the hook folder on the HDIO server:

```javascript
// hook.js

/**
 * Hook function for customizing the HDML document.
 * @param {Element} dom - The HTML element representing the HDML document.
 * @param {Object} scope - The context taken from the HTTP token of the initial request, typically containing authentication details.
 * @returns {Element} - The modified or unmodified HTML element.
 */
export function hook(dom, scope) {
  // Add your custom logic here, utilizing the provided `dom` and
  // `scope` parameters. For example, you can conditionally modify
  // the document based on the authenticated user's context.

  // Return the modified or unmodified HTML element
  return dom;
};
```

### Instructions

1. **Create a Hook Function File:**
   - Write a hook function named `hook` in a file.
   - Export the function to make it accessible.

2. **Place the File in the Hook Folder:**
   - Save the hook function file in the specific hook folder on the HDIO server.

3. **Leverage Authentication Context:**
   - Utilize the `scope` parameter to access the authentication context from the HTTP token.
   - Make decisions or perform actions based on the authenticated user's context.

### Benefits

1. **Dynamic Document Customization:**
   - Hook functions enable real-time customization of HDML documents based on the current user's authentication context.

2. **Secure Server-Side Execution:**
   - Hook functions execute on the server side within a secure context, ensuring that sensitive data and operations are handled securely.

3. **Authentication-Driven Modifications:**
   - Leverage the authentication context (`scope`) to drive document modifications based on the user's identity or permissions.

4. **Decentralized Configuration:**
   - By storing hook functions in a dedicated folder, configuration remains decentralized, making it easier to manage and update individual customization logic.

5. **No Registration Required:**
   - There's no need to register hook functions explicitly. Placing the file in the designated folder makes it automatically accessible to the HDIO server.

### Considerations

- **Security:** Ensure that the hook function handles external data securely, especially when making decisions based on the authentication context.

- **Hook Folder:** Make sure to place the hook function file in the designated hook folder on the HDIO server for automatic discovery.

By following these instructions, users can create and deploy hook functions to tailor HDML documents based on the authentication context, providing a flexible and secure customization mechanism.

# HDML WebComponents

## `hdml-include`

The `<hdml-include>` component is used to include external HDML documents within another document. It serves as a mechanism to link separate documents and merge their content during processing on the HDIO (Hyperdata Input-Output) server.

### Syntax

```html
<hdml-include
   path="/path/to/external-document.hdml">
</hdml-include>
```

### Attributes
- `path` (Required): The path to the external HDML document on the HDIO (Hyperdata Input-Output) server that you want to include.

### Example:

Assuming you have a hidden document with connection definitions named `connections.hdml`:

```html
<!-- connections.hdml - Hidden Document with Connection Definitions -->
<hdml-connection
  name="connection-1"
  type="postgres"
  host="localhost"
  user="user1"
  password="pass1"
  ssl="true"
  meta="Connection 1">
</hdml-connection>

<!-- Add more connections as needed -->
```

You can use the `hdml-include` component in your main document:

```html
<!-- main-models.hdml - Main Document with Model Definitions -->
<hdml-include
   path="/path/to/connections.hdml">
</hdml-include>

<!-- Define models with references to named connections -->
<hdml-model
   name="Model1">
   <hdml-table
      name="`connection-1`.`public`.`users`">

      <!-- ... fields ... -->

   </hdml-table>
</hdml-model>

<!-- Add more models and components as needed -->
```

## `hdml-connection`

The `hdml-connection` component represents a connection to a database. It is used to define the connection details for various database types.

### Attributes:

- `name` (Required): The name of the connection.
- `type` (Required): The type of the database. Available types are:

  - `postgres`
  - `mysql`
  - `mssql`
  - `mariadb`
  - `oracle`
  - `clickhouse`
  - `druid`
  - `ignite`
  - `redshift`
  - `bigquery`
  - `googlesheets`
  - `elasticsearch`
  - `mongodb`
- `meta` (Optional): Additional metadata or description for the connection.

The `hdml-connection` component allows you to specify the `name` and `type` attributes to identify and categorize your database connections. Choose the appropriate type based on the database system you are connecting to.

### Postgres, MySQL, MS SQL, MariaDB, Oracle, ClickHouse, Druid, Ignite, Redshift Parameters:

For the following database types: Postgres, MySQL, MS SQL, MariaDB, Oracle, ClickHouse, Druid, Ignite, Redshift, you can use the following common parameters:

| Attribute      | Description                                           | Required | Default |
|----------------|-------------------------------------------------------|----------|---------|
| `ssl`          | A boolean value indicating whether SSL is enabled.   | No       | false       |
| `host`         | The host address of the database.                     | Yes      | -       |
| `user`         | The username for authentication.                     | Yes      | -       |
| `password`     | The password for authentication.                     | Yes      | -       |
| `meta`         | Additional metadata or description for the connection. | No    | -       |

### BigQuery Parameters:

For the `bigquery` database type, you can use the following parameters:

| Attribute          | Description                                           | Required | Default |
|--------------------|-------------------------------------------------------|----------|---------|
| `project-id`       | The ID of the Google Cloud Platform (GCP) project.    | Yes      | -       |
| `credentials-key`  | The path to the JSON file containing GCP credentials. | Yes      | -       |
| `meta`             | Additional metadata or description for the connection. | No    | -       |

### Google Sheets Parameters:

For the `googlesheets` database type, you can use the following parameters:

| Attribute          | Description                                           | Required | Default |
|--------------------|-------------------------------------------------------|----------|---------|
| `sheet-id`         | The ID of the Google Sheets document.                 | Yes      | -       |
| `credentials-key`  | The path to the JSON file containing Google API credentials. | Yes | - |
| `meta`             | Additional metadata or description for the connection. | No    | -       |

### Elasticsearch Parameters:

For the `elasticsearch` database type, you can use the following parameters:

| Attribute      | Description                                           | Required | Default |
|----------------|-------------------------------------------------------|----------|---------|
| `host`         | The host address of the Elasticsearch server.          | Yes      | -       |
| `port`         | The port number on which Elasticsearch is running.    | No       | 9200    |
| `user`         | The username for authentication.                     | No       | -       |
| `password`     | The password for authentication.                     | No       | -       |
| `ssl`          | A boolean value indicating whether SSL is enabled.   | No       | false       |
| `region`       | The region where the Elasticsearch server is located. (Required for AWS deployment) | Yes | - |
| `access-key`   | The access key for AWS authentication.                | No       | -       |
| `secret-key`   | The secret key for AWS authentication.                | No       | -       |
| `meta`         | Additional metadata or description for the connection. | No    | -       |

### MongoDB Parameters:

For the `mongodb` database type, you can use the following parameters:

| Attribute      | Description                                           | Required | Default |
|----------------|-------------------------------------------------------|----------|---------|
| `host`         | The host address of the MongoDB server.               | Yes      | -       |
| `port`         | The port number on which MongoDB is running.          | Yes      | 27017   |
| `user`         | The username for authentication.                     | Yes      | -       |
| `password`     | The password for authentication.                     | Yes      | -       |
| `ssl`          | A boolean value indicating whether SSL is enabled.   | No       | false       |
| `schema`       | The name of the MongoDB collection containing schema information. | Yes | - |
| `meta`         | Additional metadata or description for the connection. | No    | -       |

### Example:

#### Without Environment Variables

```html
<hdml-connection
   name="myDatabase"
   type="postgres"
   host="example.com"
   user="user"
   password="pass">
</hdml-connection>
```

In this example, the PostgreSQL connection is configured directly within the HDML document without utilizing environment variables.

#### With Environment Variables

```html
<hdml-connection
   name="myDatabase"
   type="postgres"
   host="${DB_HOST}"
   user="${DB_USER}"
   password="${DB_PASSWORD}">
</hdml-connection>
```

In this example, environment variables are used to parameterize the PostgreSQL connection configuration. The `${DB_HOST}`, `${DB_USER}`, and `${DB_PASSWORD}` placeholders are placeholders that will be replaced with the actual values from the environment when the HDML document is processed.

Make sure that the environment variables (`DB_HOST`, `DB_USER`, `DB_PASSWORD`) are set appropriately before processing the HDML document.

These examples showcase how you can configure database connections both directly and using environment variables within the `hdml-connection` component. Adjust the attribute values according to your specific database setup and requirements.

## `hdml-model`

The `hdml-model` component represents a data model within the HDML structure. It serves as a container for organizing various tables, joins, and relationships that define the structure of the data.

## Attributes:

- `name` (Required): The name of the data model.
- `meta` (Optional): Additional metadata or description for the model.

## Example:

```html
<hdml-model
   name="EmployeeData">
   <!-- Tables within the data model -->
   <hdml-table
      name="employee"
      type="table"
      identifier="`database`.`schema`.`employee_table`">
      
      <!-- ... fields ... -->

   </hdml-table>

   <hdml-table
      name="department"
      type="query"
      identifier="SELECT * FROM `database`.`schema`.`department_table`">
      
      <!-- ... fields ... -->
   
   </hdml-table>

   <!-- Join tables using hdml-join -->
   <hdml-join
      type="inner"
      left="employee"
      right="department">

      <!-- Additional join configuration -->

   </hdml-join>

  <!-- Additional tables, joins, and relationships can be added here -->

</hdml-model>
```

In this example, the **`hdml-model`** component named "EmployeeData" contains tables (hdml-table) and a join (hdml-join) that define the data structure. Adjust the attributes and content based on your specific data modeling requirements.

## `hdml-table`

The `hdml-table` component represents a table within a data model in the HDML structure. It provides a way to define and organize data tables, whether they are actual tables, views, or materialized views from a database or the result of a SQL query.

### Attributes:

- `name` (Required): The name of the table.
- `type` (Required): The type of the table, either "table" for an actual database table, view, or materialized view, or "query" for a table generated from a SQL query.
- `identifier` (Required): The unique identifier for the table, which should include the full three-tier table name for database tables, views, or materialized views, enclosed in back quotes.
- `meta` (Optional): Additional metadata or description for the table.

### Example:

```html
<hdml-table
   name="employee"
   type="table"
   identifier="`database`.`schema`.`employee_table`">

   <!-- ... fields ... -->

</hdml-table>

<hdml-table
   name="department"
   type="table"
   identifier="`database`.`schema`.`department_view`">

   <!-- ... fields ... -->

</hdml-table>

<hdml-table
   name="sales_data"
   type="query"
   identifier="SELECT * FROM `database`.`schema`.`sales_materialized_view`">

   <!-- ... fields ... -->

</hdml-table>
```

In these examples, the **`hdml-table`** components represent a database table named "employee," a database view named "department," and a materialized view named "sales_data." Adjust the attributes and content based on your specific data modeling requirements.

## `hdml-field`

The `hdml-field` component represents a field within an `hdml-table` in the HDML context.

### Attributes:

- `name` (Required): The name of the field in the HDML context.
- `origin` (Optional): The name of the original field in the database if used within the scope of `hdml-table`, or in the parent structure if used within the scope of an `hdml-frame`. If omitted, it is assumed to be the same as the HDML field name.
- `type` (Optional): The data type of the field in the HDML context. Supported types include: `int-8`, `int-16`, `int-32`, `int-64`, `uint-8`, `uint-16`, `uint-32`, `uint-64`, `float-16`, `float-32`, `float-64`, `binary`, `utf-8`, `decimal`, `date`, `time`, `timestamp`.
- `nullable` (Optional, default: `false`): Specifies whether the field can contain null values.
- `clause` (Optional): An SQL clause defining the field. It takes precedence over the `origin` attribute. For example, ```clause="concat(`table_catalog`, '-', `table_schema`, '-', `table_name`)"```.
- `agg` (Optional, applicable in `hdml-frame`): Specifies an aggregation function for the field. Supported functions include: `none`, `count`, `countDistinct`, `countDistinctApprox`, `sum`, `avg`, `min`, `max`.
- `asc` (Optional, applicable in `hdml-frame`): Specifies the type of sorting for the field. Use `true` for ascending and `false` for descending.
- `meta` (Optional): Additional metadata or description for the field.

#### Additional Attributes for `type="decimal"`:

- `scale` (Optional): The number of decimal places.
- `precision` (Optional): The total number of digits, including both the integer and fractional parts.
- `bit-width` (Optional): The bit width of the decimal field. Supported values include `128` and `256`.

#### Additional Attributes for `type="date"`:

- `unit` (Optional): Specifies the date format unit. Supported values include `second` and `millisecond`.

#### Additional Attributes for `type="timestamp"`:

- `unit` (Optional): Specifies the timestamp format unit. Supported values include `second`, `millisecond`, `microsecond`, and `nanosecond`.
- `timezone` (Optional): Specifies the timezone for the timestamp field.

### Explanation

- **`origin` Attribute**: The `origin` attribute, if specified, represents the original field name in the database if used within the scope of `hdml-table`, or in the parent structure if used within the scope of an `hdml-frame`. This attribute allows you to map the HDML field to its counterpart in the database or `hdml-model`, or parent `hdml-frame`.

- **`type` Attribute**: The `type` attribute represents the data type of the field in the HDML context. It does not necessarily mirror the original data type of the field in the database. For example, you may use `type="decimal"` in HDML, even if the original field in the database is of a different numeric type. This attribute allows flexibility in defining the data type as it should be treated within the HDML structure.

### Example:

#### General example

```html
<hdml-table
   type="table"
   identifier="`database`.`schema`.`your_table`">
   <hdml-field
      name="employee_name"
      origin="emp_name"
      type="utf-8"
      nullable="false">
   </hdml-field>
   <hdml-field
      name="full_name"
      clause="concat(`first_name`, ' ', `last_name`)"
      type="utf-8">
   </hdml-field>
</hdml-table>
```

In this example, we define two fields within the `database.schema.your_table` table. The `employee_name` field maps to the `emp_name` field in the database, specifying a UTF-8 data type and a non-nullable constraint. The `full_name` field is created using an SQL `concat` clause, combining the `first_name` and `last_name` fields.

#### `type="decimal"`

```html
<hdml-table
   type="table"
   identifier="`database`.`schema`.`your_table`">
   <hdml-field
      name="price"
      type="decimal"
      scale="2"
      precision="10"
      bit-width="128"
      nullable="false">
   </hdml-field>
</hdml-table>
```

In this example, the `hdml-field` component with `type="decimal"` includes additional attributes to specify the scale, precision, and bit-width.

#### `type="date"`

```html
<hdml-table
   type="table"
   identifier="`database`.`schema`.`your_table`">
   <hdml-field
      name="event_date"
      type="date"
      unit="millisecond"
      nullable="false">
   </hdml-field>
</hdml-table>
```

In this example, the `hdml-field` component with `type="date"` includes the additional `unit` attribute to specify the date format.

#### `type="time"`

```html
<hdml-table
   type="table"
   identifier="`database`.`schema`.`your_table`">
   <hdml-field
      name="event_time"
      type="time"
      unit="microsecond"
      nullable="false">
   </hdml-field>
</hdml-table>
```

In this example, the `hdml-field` component with `type="time"` includes the additional `unit` attribute to specify the time format.

#### `type="timestamp"`

```html
<hdml-table
   type="table"
   identifier="`database`.`schema`.`your_table`">
   <hdml-field
      name="event_time"
      type="timestamp"
      unit="microsecond"
      timezone="UTC"
      nullable="false">
   </hdml-field>
</hdml-table>
```

In this example, the `hdml-field` component with `type="timestamp"` includes the additional `unit` and `timezone` attributes to specify the timestamp format and timezone.

## `hdml-join`

The `hdml-join` component is used to define joins between tables within an `hdml-model`. It provides flexibility in specifying the type of join and the conditions for the join through a combination of attributes and child components.

### Attributes:

- **`type`** (Required): Specifies the type of join. Supported values include:
  - `cross`: Produces the Cartesian product of two tables.
  - `inner`: Returns only the rows that have matching values in both tables.
  - `full`: Returns all rows when there is a match in either left or right table records.
  - `left`, `right`: Returns all rows from the left or right table and matching rows from the right or left table.
  - `full-outer`, `left-outer`, `right-outer`: Similar to `full`, `left`, and `right` but includes unmatched rows as well.

- **`left`** (Required): The name of the left table in the join. Should match the name of an `hdml-table` within the same `hdml-model`.

- **`right`** (Required): The name of the right table in the join. Should match the name of an `hdml-table` within the same `hdml-model`.

### Child Components:

- **`hdml-connective`** (Required): Serves as a logical connective for defining conditions.

  - **Attributes**:
    - **`operator`** (Required): Specifies the logical operator (`and` or `or`) for connecting multiple filter conditions.

  - **Child Components**:
    - **`hdml-filter`** (Required): Describes the conditions.

      - **Attributes**:
        - **`type`** (Required): Specifies the type of filter. Supported values include:
          - `keys`: Join based on matching key fields. (Note: Only for use under `hdml-join`)
          - `expr`: Use a custom SQL-like conditional clause.

      - **Attributes for `type="keys"`**:
        - **`left`** (Required): The name of the field in the left table.
        - **`right`** (Required): The name of the field in the right table.

      - **Attributes for `type="expr"`**:
        - **`clause`** (Required): Specifies the SQL-like conditional clause.
        - Example:
         ```html
         <hdml-filter
            type="expr"
            clause="`table1`.`field1` = `table2`.`field2`">
         </hdml-filter>
         ```
         In this example, it defines a condition based on the equality of `field1` in `table1` with `field2` in `table2`.

### Example:

```html
<hdml-join
   type="inner"
   left="table1"
   right="table2">
   <hdml-connective
      operator="and">
      <hdml-filter
         type="keys"
         left="field1"
         right="field2">
      </hdml-filter>
      <hdml-filter
         type="expr"
         clause="`table1`.`field1` = `table2`.`field2`">
      </hdml-filter>
   </hdml-connective>
</hdml-join>
```

This example demonstrates an `inner` join between `table1` and `table2` with a root `hdml-connective` connecting two conditions.

### Important Notes:

- You can't have two `hdml-connective` under the `hdml-join`. Some root connective must be there for proper connection.
- `hdml-filter` with type "keys" is specific to `hdml-join` and must not be used elsewhere.
- `hdml-connective` and `hdml-filter` will be used not only for joins.

## `hdml-connective`

The `hdml-connective` component is utilized to define logical connectives. It is commonly employed within the context of `hdml-join` conditions under `hdml-model` or within the context of `hdml-filter-by` conditions under `hdml-frame`. This component enables you to merge multiple filters using logical operators. `hdml-connective` is specifically designed for connecting multiple `hdml-filter` and other `hdml-connective` components.

### Attributes:

- **`operator`** (Required): Specifies the logical operator to connect filters. Supported values include:
  - `and`: Logical AND operator.
  - `or`: Logical OR operator.

### Example:

#### with `hdml-model`

```html
<hdml-model
   name="my_model">

   <!-- hdml-model configuration -->

   <hdml-join
      type="inner"
      left="table1"
      right="table2">
      <hdml-connective
         operator="and">
         <hdml-filter
            type="keys"
            left="field1"
            right="field2">
         </hdml-filter>
         <hdml-filter
            type="expr"
            clause="`table1`.`field3` = 'value'">
         </hdml-filter>
      </hdml-connective>
   </hdml-join>

   <!-- hdml-model configuration -->

</hdml-model>
```

#### with `hdml-frame`

```html
<hdml-frame
   name="my_frame"
   source="?hdml-model=my_model">

   <!-- hdml-frame configuration -->

   <hdml-filter-by>
      <hdml-connective
         operator="and">
         <hdml-filter
            type="expr"
            clause="`field1` = 'ABC'">
         </hdml-filter>
         <hdml-filter
            type="expr"
            clause="`field6` > 100">
         </hdml-filter>
      </hdml-connective>
   </hdml-filter-by>

   <!-- hdml-frame configuration -->

</hdml-frame>
```

## `hdml-filter`

The `hdml-filter` component is utilized to define filters within the context of `hdml-join` conditions under `hdml-model` or within the context of `hdml-filter-by` conditions under `hdml-frame`. This component allows you to specify conditions for joining tables or filtering data based on specific criteria. Filters are always specified under the `hdml-connective` element, allowing you to articulate the logical conditions for joining tables or filtering data.

### Attributes:

- `type` (Required): Specifies the type of filter. Supported values include:
   - `keys` (Used only in `hdml-join`): Indicates a filter based on key fields. Requires additional attributes: `left` and `right`.
   - `expr`: Represents a filter based on a conditional SQL-like expression. Requires additional attribute: `clause`.
   - `named`: Reserved for future use. Requires additional attributes: `name`, `field`, and `value`.

#### For `type="keys"`:

- `left` (Required): Specifies the left field for key-based filters. Should match the name of the `hdml-field` in the left table.
- `right` (Required): Specifies the right field for key-based filters. Should match the name of the `hdml-field` in the right table.

#### For `type="expr"`:

- `clause` (Required): Specifies the SQL-like conditional clause for expression-based filters. The format depends on the context:
   - Under `hdml-model` (`hdml-join`): Should be set as:
   
   ```html
   clause="`hdml-table-name`.`hdml-table-field-name` = 'string'"
   ```
   
   - Under `hdml-frame` (`hdml-filter-by`): Should be set as:
   
   ```html
   clause="`hdml-frame-field-name` = 'string'"
   ```

#### For `type="named"`:

- `name` (Required): Specifies the filter condition name. Supported values include:
   - `equals`: Field value equals the specified value.
   - `not-equals`: Field value does not equal the specified value.
   - `contains`: Field value contains the specified value.
   - `not-contains`: Field value does not contain the specified value.
   - `starts-with`: Field value starts with the specified value.
   - `ends-with`: Field value ends with the specified value.
   - `greater`: Field value is greater than the specified value.
   - `greater-equal`: Field value is greater than or equal to the specified value.
   - `less`: Field value is less than the specified value.
   - `less-equal`: Field value is less than or equal to the specified value.
   - `is-null`: Field value is null.
   - `is-not-null`: Field value is not null.
   - `between`: Field value is between two specified values.
- `field` (Required): Specifies the field name in the same way it is specified in the clause attribute:
   - Under `hdml-model` (`hdml-join`): Should be set as:
   
   ```html
   field="`hdml-table-name`.`hdml-table-field-name`"
   ```
   
   - Under `hdml-frame` (`hdml-filter-by`): Should be set as:
   
   ```html
   field="`hdml-frame-field-name`"
   ```
- `value` (Required): Specifies the value used to filter data.

### Example:

#### `type="keys"`

```html
<hdml-model
   name="my_model">

   <!-- hdml-model configuration -->

   <hdml-join
      type="inner"
      left="table1"
      right="table2">
      <hdml-connective
         operator="and">
         <hdml-filter
            type="keys"
            left="field1"
            right="field2">
         </hdml-filter>
      </hdml-connective>
   </hdml-join>

   <!-- hdml-model configuration -->

</hdml-model>
```

#### `type="expr"` under `hdml-model` (`hdml-join`)

```html
<hdml-model
   name="my_model">

   <!-- hdml-model configuration -->

   <hdml-join
      type="inner"
      left="table1"
      right="table2">
      <hdml-connective
         operator="and">
         <hdml-filter
            type="expr"
            clause="`table1`.`field1` = 'value'">
         </hdml-filter>
      </hdml-connective>
   </hdml-join>

   <!-- hdml-model configuration -->

</hdml-model>

```

#### `type="expr"` under `hdml-frame` (`hdml-filter-by`)

```html
<hdml-frame
   name="my_frame"
   source="?hdml-model=my_model">

   <!-- hdml-frame configuration -->

   <hdml-filter-by>
      <hdml-connective
         operator="and">
         <hdml-filter
            type="expr"
            clause="`field1` = 'ABC'">
         </hdml-filter>
      </hdml-connective>
   </hdml-filter-by>

   <!-- hdml-frame configuration -->

</hdml-frame>
```

#### `type="named"` under `hdml-model` (`hdml-join`)

```html
<hdml-model
   name="my_model">

   <!-- hdml-model configuration -->

   <hdml-join
      type="inner"
      left="table1"
      right="table2">
      <hdml-connective
         operator="and">
         <hdml-filter
            type="named"
            name="equals"
            field="`table1`.`field1`"
            value="'ABC'">
         </hdml-filter>
      </hdml-connective>
   </hdml-join>

   <!-- hdml-model configuration -->

</hdml-model>
```

#### `type="named"` under `hdml-frame` (`hdml-filter-by`)

```html
<hdml-frame
   name="my_frame"
   source="?hdml-model=my_model">

   <!-- hdml-frame configuration -->

   <hdml-filter-by>
      <hdml-connective
         operator="and">
         <hdml-filter
            type="named"
            name="equals"
            field="`field1`"
            value="'ABC'">
         </hdml-filter>
      </hdml-connective>
   </hdml-filter-by>

   <!-- hdml-frame configuration -->

</hdml-frame>
```

## `hdml-frame`

The `hdml-frame` component is used to create a data frame based on the provided HDML model or another HDML frame. It serves as a container for organizing and processing data within the HDML structure. This includes tasks such as filtering, aggregating, sorting, and performing calculations on the data. Essentially, the `hdml-frame` allows for manipulating and analyzing the data according to specified criteria.

### Attributes:

- `name` (Required): The name of the data frame.
- `source` (Required): Specifies the parent structure for the frame. This attribute indicates the source of the data model used to construct the frame. Depending on whether the parent structure is stored separately or within the same document, the value of this attribute varies:
   - If the parent structure is stored separately on the HDIO server, the value should be an HDIO path to the file, followed by either `hdml-model=model_name` or `hdml-frame=frame_name`. For example: `/path/to/file.hdml?hdml-model=model_name` or `/path/to/file.hdml?hdml-frame=frame_name`.
   - If the parent structure is declared within the same file, only specify `hdml-model=model_name` or `hdml-frame=frame_name` without the path and file name. For example: `?hdml-model=model_name` or `?hdml-frame=frame_name`.
- `limit` (Optional): Specifies the maximum number of rows to include in the frame.
- `offset` (Optional): Specifies the number of rows to skip before starting to include rows in the frame.

**Note:** At least one `hdml-field` must be specified within the `hdml-frame`.

**Note:** In the case of using `hdml-field` within the `hdml-frame` component scope, the `origin` attribute refers to the "parent" structure field name.

### Child Components:

- **`hdml-filter-by`** (Optional): The `hdml-filter-by` component is used to filter data within the context of an `hdml-frame`. It allows you to specify conditions to filter rows of data based on specific criteria. This component is essential for refining datasets and focusing on relevant information.
   - The `hdml-connective` element must be a child element of `hdml-filter-by` and is used to specify the logical operator for joining multiple filter conditions. 
   - Multiple `hdml-filter` elements can be included within `hdml-connective` to define complex filter conditions.
- **`hdml-group-by`** (Optional): The `hdml-group-by` component is used within the context of an `hdml-frame` to group data based on specific fields. It allows you to aggregate data based on common values in one or more fields. This component is useful for summarizing data and performing operations on grouped subsets of the dataset.
   - The `hdml-group-by` component must contain at least one `hdml-field` element to specify the field(s) by which the data will be grouped.
   - Grouped data can then be further manipulated or analyzed within the frame's scope.
- **`hdml-sort-by`** (Optional): The `hdml-sort-by` component is used within the context of an `hdml-frame` to sort data based on specific fields. It allows you to arrange data in ascending or descending order based on one or more fields. This component is useful for organizing data and presenting it in a structured manner.
   - The `hdml-sort-by` component must contain at least one `hdml-field` element to specify the field(s) by which the data will be sorted.
   - Sorted data can then be further manipulated or analyzed within the frame's scope.
   - The asc attribute of the `hdml-field` element determines whether the sorting is done in ascending (true) or descending (false) order. This attribute is meaningful only within the context of `hdml-sort-by`.

### Example:

```html
<hdml-frame
   name="my_frame"
   source="?hdml-model=my_model"
   limit="100"
   offset="10">

   <hdml-field
      name="field_name"
      origin="my_model_field_name">
   </hdml-field>
   <hdml-field
      name="field_counter"
      origin="my_model_field_name"
      agg="count">
   </hdml-field>

   <hdml-filter-by>
      <hdml-connective
         operator="and">
         <hdml-filter
            type="named"
            name="starts-with"
            field="field_name"
            value="'A'">
         </hdml-filter>
      </hdml-connective>
   </hdml-filter-by>

   <hdml-group-by>
      <hdml-field
         name="field_name">
      </hdml-field>
   </hdml-group-by>

   <hdml-sort-by>
      <hdml-field
         name="field_name"
         asc="true">
      </hdml-field>
   </hdml-sort-by>

</hdml-frame>
```

The following HDML code demonstrates the configuration of an hdml-frame component with various data processing operations:

- **Frame Definition:**

   - The `hdml-frame` component named "my_frame" is defined with the following attributes:
      - `name`: Specifies the name of the data frame.
      - `source`: Indicates the source of the data model used to construct the frame.
      - `limit`: Specifies the maximum number of rows to include in the frame.
      - `offset`: Specifies the number of rows to skip before starting to include rows in the frame.

- **Field Definitions:**

   - Two `hdml-field` components are included within the `hdml-frame`:
      - The first field, "field_name", retrieves data from the HDML model's field named "my_model_field_name".
      - The second field, "field_counter", also retrieves data from "my_model_field_name" but applies an aggregation function to count the occurrences of each value.

- **Data Filtering:**

   - The `hdml-filter-by` component is used to filter data based on specific criteria.
   - In this example, data is filtered to include only rows where the value of the "field_name" field starts with the letter 'A'.

- **Data Grouping:**

   - The `hdml-group-by` component is employed to group data based on the values of one or more fields.
   - Here, data is grouped based on the "field_name" field.

- **Data Sorting:**

   - The `hdml-sort-by` component is utilized to sort the data based on specified fields.
   - The data is sorted in ascending order based on the values of the "field_name" field.

This example showcases the versatility of HDML in organizing, processing, and analyzing data within a structured framework.