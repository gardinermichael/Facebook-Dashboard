# Facebook-Dashboard

<h3>Project:</h3>

Dashboard that comparatively monitors trending posts on multiple Facebook pages simultaneously, as well as news stories around the web. Created for a Facebook-focused publication repackaging stories from other outlets to publish on genre-branded Facebook pages.

<h3>Applications:</h3>

* [Google Spreadsheets](https://support.google.com/docs/answer/3093339)
  * Used for its ability to import data, perform calculations and then display the results through customizable color filters. Cloud-based and sufficient.
* [Facebook Graph API](https://developers.facebook.com/tools/explorer/)
  * Facebook allows users to make API calls through their web-based Graph API Explorer. Access tokens obtained this way are only valid for two hours unless [converted](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension) by making a server-side API call, but can be used elsewhere until then.
* [Reddit API](https://www.reddit.com/dev/api/)
  * TK
* [News API](https://newsapi.org/)
  * TK
  
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
  * Imports top 20 posts from a variety of subreddits, organized by category (i.e. Hard News: /r/News | /r/WorldNews | /r/Politics).
  * Metrics: **Reddit Score** and **Comment Count**.
  * While not different than going to a subreddit page and sorting by the top posts of the past day, having the top posts from all the subreddits you're interested in presented at the same time is a major time saver.
  * Columns: **Subreddit**, **Pub**: *Website domain*, **Score**,	**URL**: *What the post is linking to*,	**Headline**, **Time**, **Comments** and **Permalink**: *to the Reddit post*.
* newsTop
  * Pub	Headlines	Description	URL	Image URLs	Uncoverted Time	Date in Zulu	Time w/ Zulu	Time w/o Z	Time EST	EST	Converted time doesn't work

<h3>Libraries:</h3>

*  [Import JSON](https://github.com/bradjasper/ImportJSON)
  * Adds an =ImportJSON() function to the Google spreadsheet (Facebook Graph API outputs data as a JSON). Installed by manually adding ImportJSON.gs through the spreedsheet's Script Editor.

<h3>Workflow:</h3>



