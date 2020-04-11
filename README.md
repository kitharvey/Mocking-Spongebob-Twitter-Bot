# Mocking-Spongebob-Twitter-Bot
This project was made just for fun.
Copies a random tweet and tweeting it in "MoCkINg SpOngEbOB MEme" text format.
Go troll on twitter.

## Set Up
#### Initialize node package management
`npm init`
#### Install twit.js https://www.npmjs.com/package/twit
`npm install twit`
#### Create Twitter App on https://developer.twitter.com/ and Get your Twitter API



## Code
```javascript
// Mocking Spongebob Meme twitter bot
// Mocks on random tweet
// Copies a tweet and convert to Mocking Spongebob Meme Text


// npm install twit
//
const Twit = require('twit')


// importing API tokens from another file
// consumer_key:         '...',
// consumer_secret:      '...',
// access_token:         '...',
// access_token_secret:  '...',
//
const token = require('./token')


//
// creating new to object to connect the API tokens to installed twit package
const T = new Twit(token)



setInterval(streaming, 1000*60*10) //setting stream interval every 10 mins
streaming() // reading and selecting tweets 
replyStream() // reading and replying replies


// Choosing random tweet -----------------
function streaming() {
  //
  // setting bound box of a country
  // source 'graydon/country-bounding-boxes.py'
  //
  let PH = [117.17427453, 5.58100332277, 126.537423944, 18.5052273625] 
  let stream = T.stream('statuses/filter', { locations: PH }) // setting filter to selected country

  stream.on('tweet', tweets) // streaimng filtered tweets

  function tweets(twts) {

    let name = twts.user.screen_name // copying the username who posted the tweet
    let txt = twts.text // copying the tweet
    let ID = twts.id_str // copying the tweet ID
    let count = 0 // setting my tweet counter
    
    
    if (twts.text.length < 100){ // filtering tweets with less then 100 characters, I hate long tweets
      if(!txt.includes('https://') && !txt.includes('@')) { // filtering tweets with media and menstioned name, to avoid retweeting links
  
          // calling mock function and converting the text to "MoCkiNg SpoNGebOB TeXt"
          // adding '...edi waw' to increase mocking factor
          txt = mock(txt + ' ...edi waw') 

          T.post('statuses/update', { status: `@${name}\n${txt}` }, function(err, data, response) { // posting a tweet mentioning the random person with their converted tweet
            if (err) console.log(err)
            else console.log('tweeted!') // check if successful
          })
          like(ID) // licking the chosen tweet by the tweet ID
          count ++ // my tweet counter increment
          console.log(count)
          if (count === 1) stream.stop() // limiting the activity with 1 post, then set the setInterval to repeat the process
        
        
      }
    }
  }
}
  

  


  // Reply -----------------
  function replyStream() {
    let my_stream = T.stream('statuses/filter', { track: '@your_username' }); // filtering tweets related to your account
    my_stream.on('tweet', replies) // streaming filtered tweets

    function replies(twts) {
    let name = twts.user.screen_name // copying the username who posted the tweet
    let txt = twts.text // copying the tweet
    let reply_to = twts.in_reply_to_screen_name // copying the username whom reply was sent
    let ID = twts.id_str // copying the reply tweet ID
  
    if (reply_to === 'your_username') { // filtering if there are replies to you

      // removing your username on the tweet
      // calling mock function and converting the text to "MoCkiNg SpoNGebOB TeXt"
      // adding 'nye nye nye nye' to increase mocking factor
      txt = mock(txt.replace(/@your_username/g,'') + ' nye nye nye nye')
      T.post('statuses/update', { status: `@${name}\n${txt}`, in_reply_to_status_id:'' + ID}, function(err, data, response) { // posting the reply
        if (err) console.log(err)
        else console.log('tweeted!') // check if successful
      })
      like(ID) // licking the reply tweet by the reply tweet ID
      }
    }
  }
  
  

  // Mocking Spongebob Function
  function mock(txt) {
    let text = txt.toLowerCase() // setting all characters to lowercase
    let res = ""
    
    for(let i = 0; i < text.length; ++i) { // looping the characters
        let rand = Math.floor(Math.random() * 2) // setting random number 0 OR 1
        if(rand === 0) { // if 0 convert the letter to uppercase
            res += text.charAt(i).toUpperCase()
        } 
        else { // leave as is
            res += text.charAt(i)
        }
    }
    return res
  }

  // like the tweet
  function like(ID){
    T.post('favorites/create', { id:'' + ID }, function(err, data, response) {
      if (err) console.log(err)
      else console.log('liked!')})
  }

```
