import azure.functions as func
import json
import os
from azure.data.tables import TableServiceClient

# CosmosDB connection
CONNECTION_STRING = os.getenv("AzureWebJobsStorage")
TABLE_NAME = "visitorCount"

def main(req: func.HttpRequest) -> func.HttpResponse:
    service_client = TableServiceClient.from_connection_string(CONNECTION_STRING)
    table_client = service_client.get_table_client(TABLE_NAME)

    entity = table_client.get_entity(partition_key="count", row_key="1")
    current_count = entity["visits"]
    
    # Increment visitor count
    entity["visits"] = current_count + 1
    table_client.update_entity(mode="Replace", entity=entity)

    return func.HttpResponse(json.dumps({"visits": entity["visits"]}), mimetype="application/json")
