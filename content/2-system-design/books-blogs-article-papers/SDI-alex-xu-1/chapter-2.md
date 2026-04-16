
- reading from disk is more expensive than from network.

## power of two : 

2^10 bytes=  1kb
2^20 = 1mb
2^30 = 1gb
2^40 = 1tb


## estimate twitter QPS and storage requirement : 

Assumption :
- 300 million monthly users
- 50% of users use Twitter daily
- Users post 2 tweets per day on average
- 10% of the tweet contains media
- Data is stored for 5 yrs

Estimates : 
- QPS estimate: 
	- Daily active users (DAU) = `300 mil * 50% = 150 mil`
	- QPS = `DAU * 2 / 24 / 3600` = `150mil * 2 /24/3600` = ~3500
	- Peek QPS = 2*QPS = ~7000

- Estimate media storage : 
	- Average tweet size : 
		- tweet_id = 64 bytes
		- text = 140 bytes
		- media = 1mb
	- Estimates : 
		- media storage : 150 mil * 2 * 10% * 1mb = 30TB per day
		- 5 yrs backup : 30 TB * 365 * 5  = ~55PB

