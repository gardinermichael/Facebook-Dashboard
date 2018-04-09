# Facebook (& More) Dashboard

<h3>Project:</h3>

Dashboard that comparatively monitors trending posts on multiple Facebook pages simultaneously, as well as news stories around the web. Created for a Facebook-focused publication repackaging stories from other outlets and republishing on genre-branded Facebook pages.

<h3>Applications:</h3>

* [Google Spreadsheets](https://support.google.com/docs/answer/3093339)
  * Used for its ability to import data, perform calculations and then display the results through customizable color filters. Cloud-based and sufficient.
* [Facebook Graph API](https://developers.facebook.com/tools/explorer/)
  * Facebook allows users to make API calls through their web-based Graph API Explorer. Access tokens obtained this way are only valid for two hours unless [converted](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension) by making a server-side API call, but can be used elsewhere until then.
* [Reddit API](https://www.reddit.com/dev/api/)
  * For the purposes of this dashboard, is mainly used to retrieve a subreddit's top posts within a certain time range. Does allow a generous amount of information to be retrieved from Reddit.
* [News API](https://newsapi.org/)
  * Service handles scraping and outputting data about news stories from [dozens of publications](https://newsapi.org/sources). Paid API service for any commercial entity, but free for developmental and non-commercial use. Recent capabilities added in API Version 2 greatly expanded the service's value, including finding the top stories for certain categories (i.e. business) or topic (i.e. Apple). 
 
<h3>Features</h3>

* facebookGenerator
  * Imports the engagement and metadata for a Facebook Page's last 30 published stories.
  * Metrics to compare stories (in order of importance): **Shares**, **Reactions** and **Comments**.
  * Metrics to compare stories across pages: **Time** since posting, a page's **Audience** count (users who recently engaged with the page) and a page's **Fan** count (users who have *Liked* or *Followed* a page).
  * Story stats are compared across different pages by dividing the share, reaction and comment counts by the time, audience and fan counts. 
  * While the numbers themselves are meaningless for the most part, the results are stratified into three categories: **Green** for well-performing stories, **Yellow** for an average performance and **Red** for poor performance.
  * Due to the nature of Facebook, a certain story may perform well in one category, but dismally in another, and still be a worthwhile lead. All the information the dashboard outputs needs to contextualized and weighted by the user.
  * Columns: **Publication**,	**Fan/Audience Ratio**,	**Total Shares**: *Story's total shares on Facebook --	meaning not just the instance on the Page*, **Message**: *Short blurb accompanying every Facebook post -- not the description a post imports from the website*, **Link**: *to the story*,	**FB Link**: *Link to the story's Facebook post*, **Date**: *in UTC*, **Type**: *i.e. post, image, video*,	**Headline**, **Shares**: *for that post*,	**Reactions**,	**Comments**, **Time**: *Date and time posted*, **H**: *Hours since story was posted*, **M**: *Minutes since story was posted*,	**TS**: *Shares weighted by time*, **TR**: *Reactions weighted by time*,	**TC**: *Comments weighted by time*, **EST Timezone**: *Timezone of user*, **Audience**: *of Facebook Page*,	**WS**: *Shares weighted by Audience*,	**WR**: *Reactions weighted by Audience*,	**WC**: *Comments weighted by Audience*, **Fans**: *of Facebook page*,	**WS**: *Shares weighted by Fans*,	**WR**: *Reactions weighted by Fans* and	**WC**: *Comments weighted by Fans*. 
* redditGenerator
  * Imports top 20 posts for the past day from a variety of subreddits, organized by category: *i.e. Hard News - /r/News | /r/WorldNews | /r/Politics*.
  * Metrics: **Reddit Score** and **Comment Count**.
  * While no different than going to a subreddit page and sorting by top posts for the past day, cannot be understated how much time is saved by having all top posts from different subreddits presented in the same view.
  * Columns: **Subreddit**, **Pub**: *Website domain*, **Score**,	**URL**: *What the post is linking to*,	**Headline**, **Time**, **Comments** and **Permalink**: *to the Reddit post*.
* newsTopGenerator
  * Imports top stories from a variety of publications, organized by category: *i.e. Science Magazines -- National Geographic | New Scientist*.
  * No ranking or comparisons other than being the top 8, but again, having stories presented simultaneously immensely speeds up daily news digest. 
  * Columns: **Publications**, **Headlines**,	**Description**, **URL**, **Image URLs** and **Time**.

<h3>Libraries:</h3>

*  [Import JSON](https://github.com/bradjasper/ImportJSON) 
   * Adds an =ImportJSON() function to the Google spreadsheet. Facebook Graph API outputs data as a JSON, as does almost every API. Installed by manually adding ImportJSON.gs through the spreedsheet's Script Editor.

<h3>Workflow:</h3>

* Set-up:
  * Generator pages display the output of the API calls, while the support pages act as a database for assembling them.
* Spreadsheet Commands
  * API Calls are assembled by the [=CONCATENATE()](https://support.google.com/docs/answer/3094123?hl=en) command.
  * Publication names and/or etcetera can be automatically generated by using the [=SPLIT()](https://support.google.com/docs/answer/3094136) command.
* Facebook
  * Support sheet labeled: codeLab
    * API Token is placed into Cell A1. Access tokens are copied down from here into Column S for use in each row.
    * Facebook page URLs are inputted into Column A.
    * Column B splits those URLs to extract the publication names. With those names: 
      * Column C finds the Facebook Page IDs.
        * `"/id"`
        * Example: *`=importjson("https://graph.facebook.com/PUBLICATION-NAME/?access_token=***************)", "/id","noInherit,noTruncate,noHeaders")`*
      * Column D and E finds the Fan and Audience counts.
        * `"/likes,/talking_about_count"`
        * Example: *`=importjson("https://graph.facebook.com/PUBLICATION-NAME/?access_token=***************)", "/likes,/talking_about_count","noInherit,noTruncate,noHeaders")`*
    * Parameter requirements for generating data about recent posts are included in Column F and so on.
      * `/data/message,/data/link,/data/permalink_url,/data/created_time,/data/type,/data/name,/data/shares/count	/data/reactions/summary/total_count,/data/comments/summary/total_count`
  * Generator page depends on the API token in codeLab.
    * API call per publication. Example:
      * *`=ImportJSON("https://graph.facebook.com/v2.8/",C3,"/posts/?fields=message,link,permalink_url,created_time,type,name,id,comments.limit(0).summary(true),shares,likes.limit(0).summary(true),reactions.limit(0).summary(true)&limit=30&date_format=U&access_token=************************", "data/message,/data/link,/data/permalink_url,/data/created_time,/data/type,/data/name,/data/shares/count	/data/reactions/summary/total_count,/data/comments/summary/total_count", "noInherit,noTruncate,noHeaders")`*
    * Seperate API call for finding a story's total shares. Example:
      * *`=importJSON("https://graph.facebook.com/v2.8/POST?access_token=**************", "/share/share_count", "noInherit,noTruncate,noHeaders")`*
    * Weighted Categories are found by dividing that number (Shares/Reactions/Comments) by either Time, Fan, or Audience count. The outputted numbers are arbitary, but the results stratified to compare and contrast between publications. 
* Reddit
  * No support sheet.
  * Generator page invokes the API call with these parameters:
    * `"/data/children/data/domain,/data/children/data/subreddit,/data/children/data/score,/data/children/data/permalink,/data/children/data/url,/data/children/data/title,/data/children/data/created_utc,/data/children/data/num_comments"`
  * Example:
    * *`=importJSON("https://www.reddit.com/r/news/top/.json?count=20&t=day", "/data/children/data/domain,/data/children/data/subreddit,/data/children/data/score,/data/children/data/permalink,/data/children/data/url,/data/children/data/title,/data/children/data/created_utc,/data/children/data/num_comments", "noInherit,noTruncate,noHeaders")`*
* News Stories
  * Support sheet labeled: newsSources
    * Cell A1 generates a list of the sources in News API. Output includes IDs, publication names, categories and locations.
    * Cell Column E takes the publication IDs and assembles the API call per each publication.
  * Generator page invokes the assembled API calls from newsSources!E* and outputs the results in the organized categories.
    * Parameters include: `"/articles/title,/articles/description,/articles/url,/articles/publishedAt"`
  * Example:
    * *`=importJSON("https://newsapi.org/v1/articles?source=bbc-news&sortBy=top&apiKey=**************************","/articles/title,/articles/description,/articles/url,/articles/publishedAt","noInherit,noTruncate,noHeaders")`*



