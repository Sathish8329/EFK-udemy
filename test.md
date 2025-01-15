# Steps to View Logs in Kibana Dashboard

Follow these steps to explore and view logs from all pods and containers in the Kibana dashboard:

## 1. Open Kibana Dashboard
- Open your Kibana dashboard in your web browser.

## 2. Access Management Settings
1. Click the menu icon (three lines) in the top-left corner of the dashboard.
2. Navigate to **Management** > **Stack Management**.

## 3. Create an Index Pattern
1. Under **Kibana**, select **Index Patterns**.
2. Click the **Create Index Pattern** button.
3. Enter a name for the index pattern, e.g., `*` (wildcard to match all indices).
4. For the **Timestamp field**, select `@timestamp`.
5. Click **Create Index Pattern** to save your configuration.

## 4. Discover Logs
1. Click the menu icon (three lines) again in the top-left corner.
2. Go to **Discover**.
3. Here, you will see logs from all pods and containers based on your index pattern.

## Notes
- Ensure your Elasticsearch indices have the required data and mappings for the `@timestamp` field to avoid issues while creating the index pattern.
- Use filters and search queries in the **Discover** section to refine the logs displayed.

Happy logging! 🎉
