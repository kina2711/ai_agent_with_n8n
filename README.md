# Automated YouTube Channel Metrics Pipeline (n8n + AI-Agent)

This n8n workflow lets you monitor multiple YouTube channels (grouped by “teams”) and automatically collect, enrich, and store video-level metrics into a Google Sheet. Under the hood it:

1. **Manual Trigger**  
   Starts when you click **Execute Workflow** in the n8n editor.

2. **Team Configuration**  
   Six **Set Team** nodes define different groups (e.g. Team 1, Team 2, Team SEO, Team Cộng đồng, etc.) along with their corresponding YouTube channel URLs.

3. **Channel Lookup & Playlists**  
   For each team:  
   - An **HTTP Request** to the YouTube Data API resolves the channel’s handle → returns `contentDetails.relatedPlaylists.uploads`.  
   - A **YouTube → getAll (playlistItem)** node fetches all video IDs from the channel’s upload playlist.

4. **Video Details Enrichment**  
   For each video ID:  
   - A **YouTube → get (video)** node pulls metadata (title, published date).  
   - A **Set** node then maps into a uniform schema:  
     - **Team**, **Platform** (“YouTube”), **Channel Name**, **Channel URL**, **Channel Creation Date**  
     - **Video URL**, **Published Date** (and month), **Title**, **View/Like/Comment** counts

5. **Merging & Aggregation**  
   Merge nodes collate all teams’ streams of enriched records into one unified data output.

6. **Destination: Google Sheets**  
   The final **Google Sheets → appendOrUpdate** node writes/appends each row into a specified spreadsheet and tab, so you can immediately visualize trends or build dashboards.

---

## Key Benefits

- **Hands-off data refresh**  
  Just click **Test workflow** to pull the latest stats across all monitored channels.

- **Scalable to more teams**  
  Simply duplicate a **Set Team → channel lookup → video branch** for new channels.

- **Centralized reporting**  
  All metrics land in a single Google Sheet for easy filtering, charting, or downstream analysis.
