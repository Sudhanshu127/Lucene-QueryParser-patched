## LuceneQueryValidator
Creating a syntactical and lexical validator for a query in elasticsearch 7.7 based on lucene 8.4.1  

### About
The code has been used from [https://github.com/apache/lucene-solr/tree/master/lucene/queryparser/src/java/org/apache/lucene/queryparser](https://github.com/apache/lucene-solr/tree/master/lucene/queryparser/src/java/org/apache/lucene/queryparser)  

### File Changes
* <strong>ParseException</strong>
    * Returns "Syntax Error" 

* <strong>TokenMgrError</strong>
    * Returns "Lexical Error" during parsing 
    * Contains a token specifying the error line, column, image
    
* <strong>QueryParserBase</strong>  
    * Handles ParseException by retrieving expected token images
    * Handles TokenMgrError by parsing into ParseException

### Using

```
import org.apache.lucene.queryparser.classic.Validate;
import org.apache.lucene.queryparser.classic.ValidateResult;
import org.apache.lucene.queryparser.classic.Mapping;

...
    String query = "(Java AND tool:Lucene)";
    Validate validate = new Validate();
    ValidateResult result = validate.validateQuery(query);
    System.out.println(result);

    Mapping mapping = new Mapping();
    mapping.checkFields = true;
    mapping.fields.add("database");
    validate.setMapping(mapping);
    result = validate.validateQuery(query);
    System.out.println(result);
...

```

#### Validate
Class that handles validating a string query.


| Function | Description | parameters | return type |
| -------- | ----------- | ---------- | ----------- |
| setMapping | set the corresponding mapping to validate upon | Mapping | void |
| validateQuery | validate the string query | String | ValidateResult |


#### ValidateResult
Result Object

| Field | Description | type |
| -------- | --------- | ---- |
| valid | Query is valid or not | boolean|
| type | Type of error (Syntax, Lexical, of Field Mismatch) | String |
| query | Input query | String |
| errorIndex | First query index where error occurred | int |  
| validToken | Last token image that was parsed correctly | String |
| errorToken | Token image that was unexpected | String |
| expectedTokens | Expected tokens | List<String> |

| Function | Description | parameters | return type |
| -------- | ----------- | ---------- | ----------- |
| toString | convert to string | | String |

##### Builder
SubClass To build ValidateResult

| Field | Description | type | default |
| -------- | --------- | ---- | ------ |
| valid | Query is valid or not | boolean | |
| type | Type of error (Syntax, Lexical, of Field Mismatch) | String | null |
| query | Input query | String | null |
| errorIndex | First query index where error occurred | int |  -1 |
| validToken | Last token image that was parsed correctly | String | null |
| errorToken | Token image that was unexpected | String | null |
| expectedTokens | Expected tokens | List<String> | new Array<List> |

| Function | Description | parameters | return type |
| -------- | ----------- | ---------- | ----------- |
| Builder | Constructor  | boolean(valid) | ValidateResult.Builder |
| type | set type | String | ValidateResult.Builder |
| query | set query | String | ValidateResult.Builder |
| errorIndex | set errorIndex | int | ValidateResult.Builder |
| validToken | set last valid token | String | ValidateResult.Builder |
| errorToken | set first error token | String | ValidateResult.Builder |
| addExpectedToken | add an expected token | String | ValidateResult.Builder |
| removeExpectedToken | remove a valid token | String | ValidateResult.Builder |
| build | convert to ValidateResult | | ValidateResult |


#### Mapping
Objects that contain fields to validate upon

| Field | Description | type | default | 
| -------- | --------- | ---- | ------- | 
| checkFields | whether to check fields or not | boolean | false |
| fields | fields to validate upon | Set<String> | |


| Function | Description | parameters | return type |
| -------- | ----------- | ---------- | ----------- |
| Mapping | default constructor | | Mapping |
| Mapping | cloning constructor | Mapping | Mapping |
| checkField | method to instruct how to validate | String | boolean |