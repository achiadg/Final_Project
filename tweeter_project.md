

```python
nltk.download()
```

    showing info https://raw.githubusercontent.com/nltk/nltk_data/gh-pages/index.xml
    




    True




```python
!pip install tweepy
```

    Collecting tweepy
      Downloading tweepy-3.5.0-py2.py3-none-any.whl
    Requirement already satisfied: six>=1.7.3 in c:\anaconda\lib\site-packages (from tweepy)
    Requirement already satisfied: requests-oauthlib>=0.4.1 in c:\anaconda\lib\site-packages (from tweepy)
    Requirement already satisfied: requests>=2.4.3 in c:\anaconda\lib\site-packages (from tweepy)
    Requirement already satisfied: oauthlib>=0.6.2 in c:\anaconda\lib\site-packages (from requests-oauthlib>=0.4.1->tweepy)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in c:\anaconda\lib\site-packages (from requests>=2.4.3->tweepy)
    Requirement already satisfied: idna<2.7,>=2.5 in c:\anaconda\lib\site-packages (from requests>=2.4.3->tweepy)
    Requirement already satisfied: urllib3<1.23,>=1.21.1 in c:\anaconda\lib\site-packages (from requests>=2.4.3->tweepy)
    Requirement already satisfied: certifi>=2017.4.17 in c:\anaconda\lib\site-packages (from requests>=2.4.3->tweepy)
    Installing collected packages: tweepy
    Successfully installed tweepy-3.5.0
    


```python
!pip install nltk
```

    Requirement already satisfied: nltk in c:\anaconda\lib\site-packages
    Requirement already satisfied: six in c:\anaconda\lib\site-packages (from nltk)
    


```python
!pip install plotly
```

    Collecting plotly
      Downloading plotly-2.2.3.tar.gz (1.1MB)
    Requirement already satisfied: decorator>=4.0.6 in c:\anaconda\lib\site-packages (from plotly)
    Requirement already satisfied: nbformat>=4.2 in c:\anaconda\lib\site-packages (from plotly)
    Requirement already satisfied: pytz in c:\anaconda\lib\site-packages (from plotly)
    Requirement already satisfied: requests in c:\anaconda\lib\site-packages (from plotly)
    Requirement already satisfied: six in c:\anaconda\lib\site-packages (from plotly)
    Requirement already satisfied: ipython_genutils in c:\anaconda\lib\site-packages (from nbformat>=4.2->plotly)
    Requirement already satisfied: traitlets>=4.1 in c:\anaconda\lib\site-packages (from nbformat>=4.2->plotly)
    Requirement already satisfied: jsonschema!=2.5.0,>=2.4 in c:\anaconda\lib\site-packages (from nbformat>=4.2->plotly)
    Requirement already satisfied: jupyter_core in c:\anaconda\lib\site-packages (from nbformat>=4.2->plotly)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in c:\anaconda\lib\site-packages (from requests->plotly)
    Requirement already satisfied: idna<2.7,>=2.5 in c:\anaconda\lib\site-packages (from requests->plotly)
    Requirement already satisfied: urllib3<1.23,>=1.21.1 in c:\anaconda\lib\site-packages (from requests->plotly)
    Requirement already satisfied: certifi>=2017.4.17 in c:\anaconda\lib\site-packages (from requests->plotly)
    Building wheels for collected packages: plotly
      Running setup.py bdist_wheel for plotly: started
      Running setup.py bdist_wheel for plotly: finished with status 'done'
      Stored in directory: C:\Users\3AFA~1\AppData\Local\pip\Cache\wheels\2c\4c\ec\eb8a05f2e5005799f802e602a47e439ae9e09844822ebc4fd0
    Successfully built plotly
    Installing collected packages: plotly
    Successfully installed plotly-2.2.3
    

we will connect to tweeter API using tweepy


```python
import tweepy
consumer_key='X52uxzboZKvontEkfsLUkcGrV'
consumer_secret='ewhRySrNg9iAt64goqU8OCPDOsWTKCqtnl0QGnj5iBlAx9EKcM'
access_token='937304105726431232-HnzJ5MhH15cJhjTs15WD7paYOzgC5f4'
access_token_secret='iyPDYJQaGKya4chysYlGC9jInrnp5Wn4A2LGm1T2cnmiK'

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

```

get 50 most recent tweets of realDonaldTrump, HillaryClinton, BarackObama, Beyonce, GalGadot.
WE will print for each one the number of tweets we got from him.


```python
alltweets = []	
curr_length = 0

alltweets.extend(api.user_timeline(screen_name = "realDonaldTrump",count=50, tweet_mode = 'extended'))
print('Donald Trump has {0} tweets'.format(len(alltweets) - curr_length))
curr_length = len(alltweets)

alltweets.extend(api.user_timeline(screen_name = "HillaryClinton",count=50, tweet_mode = 'extended'))
print('Hillary Clinton has {0} tweets'.format(len(alltweets) - curr_length))
curr_length = len(alltweets)

alltweets.extend(api.user_timeline(screen_name = "BarackObama",count=50, tweet_mode = 'extended'))
print('Barack Obama has {0} tweets'.format(len(alltweets) - curr_length))
curr_length = len(alltweets)

alltweets.extend(api.user_timeline(screen_name = "JerrySeinfeld",count=50, tweet_mode = 'extended'))
print('Jerry Seinfeld has {0} tweets'.format(len(alltweets) - curr_length))
curr_length = len(alltweets)

alltweets.extend(api.user_timeline(screen_name = "GalGadot",count=50, tweet_mode = 'extended'))
print('Gal Gadot has {0} tweets'.format(len(alltweets) - curr_length))
curr_length = len(alltweets)
```

    Donald Trump has 50 tweets
    Hillary Clinton has 50 tweets
    Barack Obama has 50 tweets
    Jerry Seinfeld has 50 tweets
    Gal Gadot has 50 tweets
    

removes all hashtags, @, and http words in order for the NLP to work better


```python
import re

def clean_text(text):
    text_updated = " ".join(filter(lambda x: '#' not in x and '@' not in x and 'http' not in x, text.split()))
    return re.sub("[^a-zA-Z']",   # The pattern to search for: in this case- all characters but english letters
                      " ",                   # The pattern to replace it with
                      text_updated)
```

We will create a function for stemming a sentence in order to make the words in their original meaning.
It will improve our accuracy.


```python
import nltk

porter = nltk.PorterStemmer()
lancaster = nltk.LancasterStemmer()

def stem_sentence(sentence):
    return " ".join([lancaster.stem(w) for w in sentence.split(" ")])

```

from the tweet object we will save only the text and the tweet's writer


```python
import pandas as pd
import numpy as np

TEXT_IDX = 1
NAME_IDX = 0

alltweets_name_and_text = [[tweet.user.name, clean_text(tweet.full_text)] for tweet in alltweets]

alltweets_text = [stem_sentence(tweet[TEXT_IDX]) for tweet in alltweets_name_and_text]
alltweets_name = [tweet[NAME_IDX] for tweet in alltweets_name_and_text]
alltweets_names_as_df = pd.DataFrame({'Name' : np.array(alltweets_name)})

print(alltweets_text)
```

    [' you had hil clinton and the democr party try to hid the fact that they gav money to gps fus to cre a dossy which was us by their al in the obam admin to convint a court mislead  by al account  to spy on the trump team   tom fitton  jw', 'the top lead and investig of the fbi and the just depart hav polit the sacr investig process in fav of democr and against republ   someth which would hav been unthink just a short tim ago  rank  amp  fil ar gre peopl ', 'the democr just ar t cal about dac  nant pelos and chuck schumer hav to get mov fast  or they ll disappoint you again  we hav a gre chant to mak a deal or  blam the dem  march  th is com up fast ', 'thank you for al of the nic comply and review on the stat of the un speech       mil peopl watch  the highest numb in hist  beat every oth network  for the first tim ev  with      mil peopl tun in  del from the heart ', 'march  th is rapid approach and the democr ar doing noth about dac  they resist  blam  complain and obstruct   and do noth  start push nant pelos and the dem to work out a dac fix  now ', 'head to beauty west virgin to be with gre memb of the republ party  wil be plan infrastruct and discuss immigr and dac  not easy when we hav no support from the democr  not on dem vot for our tax cut bil  nee mor republ in     ', 'join me liv for the', 'congrat to americ s new secret of alex az ', 'our econom is bet than it has been in many decad  busy ar com back to americ lik nev bef  chrysler  as an exampl  is leav mexico and com back to the us  unemploy is near record low  we ar on the right track ', 'somebody pleas inform jay z that becaus of my policy  black unemploy has just been report to be at the lowest rat ev record ', 'democr ar not interest in bord saf  amp  sec or in the fund and rebuild of our milit  they ar on interest in obstruct ', 'i hav off dac a wond deal  includ a doubl in the numb of recipy  amp  a twelv year pathway to cit  for two reason      becaus the republ want to fix a long tim terr problem      to show that democr do not want to solv dac  on us it ', 'talib target innoc afgh  brav pol in kab today  our thought and pray go to the victim  and first respond  we wil not allow the talib to win ', 'on holocaust remembr day we mourn and griev the murd of   mil innoc jew men  wom and childr  and the mil of oth who per in the evil naz genocid  we pledg with al of our might and resolv  nev again ', 'thank you to brandon jud of the nat bord patrol council for his strong stat on that we very bad nee the wal  must also end loophol of  catch  amp  releas  and cle up the leg and oth proc at the bord now for saf  amp  sec reason ', 'thank you for the wond welcom', 'dac has been mad increas difficult by the fact that cryin  chuck schumer took such a beat ov the shutdown that he is un to act on immigr ', 'head back from a very excit two day in davo  switzerland  speech on americ s econom rev was wel receiv  many of the peopl i met wil be invest in the u s a  ', 'join me liv at the      world econom for in davo  switzerland ', 'gre bil meet with presid of the swiss conf   as we continu to strengthen our gre friend  such an hon to be in switzerland ', 'it was an hon to meet with republ of rwand presid paul kagam thi morn in davo  switzerland  many gre discuss ', 'wil be interview on by com up at     am from davo  switzerland  enjoy ', 'today  am everywh rememb the brav men and wom of who lost their liv in our nat s etern quest to expand the bound of hum pot ', 'very produc bil meet with prim min benjamin of israel   in davo  switzerland ', 'gre bil meet with prim min theres may of the unit kingdom  affirm the spec rel and our commit to work togeth on key nat sec challeng and econom opportun ', 'wil soon be head to davo  switzerland  to tel the world how gre americ is and is doing  our econom is now boom and with al i am doing  wil on get bet   our country is fin win again ', 'it was my gre hon to welcom may s from across americ to the wh  my admin wil alway support loc govern   and list to the lead who know their commun best  togeth  we wil ush in a bold new er of peac and prosp ', 'ear today  i spok with of kentucky regard yesterday s shoot at marshal county high school  my thought and pray ar with bailey holt  preston cop  their famy  and al of the wound victim who ar in recovery  we ar with you ', 'tremend invest by company from al ov the world being mad in americ  ther has nev been anyth lik it  now disney  j p  morg chas and many oth  mass reg reduc and tax cut ar mak us a powerh again  long way to go  job  job  job ', 'cryin  chuck schumer ful understand  espec aft his humy def  that if ther is no wal  ther is no dac  we must hav saf and sec  togeth with a strong milit  for our gre peopl ', 'wher ar the        import text mess between fbi lov lis pag and pet strzok  blam samsung ', 'thank you to gen john kel  who is doing a fantast job  and al of the staff and oth in the whit hous  for a job wel don  long hour and fak report mak yo job mor difficult  but it is alway gre to win  and few hav won mor than us ', 'nobody know for sur that the republ  amp  democr wil be abl to reach a deal on dac by febru    but everyon wil be try    with a big addit foc put on milit strength and bord sec  the dem hav just learn that a shutdown is not the answ ', 'in on of the biggest story in a long tim  the fbi now say it is miss fiv month wor of lov strzok pag text  perhap         and al in prim tim  wow ', 'ev crazy jim acost of fak new cnn agr   trump world and wh sourc dant in end zon  trump win again   schumer and dem cav   gambl and lost   thank you for yo honesty jim ', 'big win for republ as democr cav on shutdown  now i want a big win for everyon  includ republ  democr and dac  but espec for our gre milit and bord sec  should be abl to get ther  see you at the negoty tabl ', 'end the democr obstruct ', 'democr hav shut down our govern in the interest of their far left bas  they don t want to do it but ar powerless ', 'the democr ar turn down serv and sec for cit in fav of serv and sec for non cit  not good ', 'thank you to brad blakem on for grad year on of my presid with an  a  and likew to doug schoen for the very good grad and stat  work hard ', 'gre to see how hard republ ar fight for our milit and saf at the bord  the dem just want illeg immigr to pour into our nat uncheck  if stalem continu  republ should go to      nuclear opt  and vot on real  long term budget  no c r  s ', 'rt democr ar hold our milit host ov their desir to hav uncheck illeg immigr  can t let that hap ', "rt  my fath was elect for on reason  and that's becaus he act believ in put americ first  which is ", 'rt  sint took off          new job wer fil by wom  ov half a mil am wom hav en ', "rt  let's look at the calend  it's janu   th  dac expir on march  th  that mean thi was a construct of ", "rt  peopl hav seen a year that's incred  that's been fil with noth but the best for our country  americ ", 'er trump on on now ', 'the trump admin has termin mor unnecess reg  in just twelv month  than any oth admin has termin dur their ful term in off  no mat what the leng  the good new is  ther is much mor to com ', 'rt democr ar far mor concern with illeg immigr than they ar with our gre milit or saf at our dang ', 'unprec success for our country  in so many way  sint the elect  record stock market  strong on milit  crim  bord   amp  is  jud strength  amp  numb  lowest unemploy for wom  amp  al  mass tax cut  end of individ mand   and so much mor  big      ', ' ', 'i wrot a facebook post about a decid i mad    year ago  what s chang   amp  on an issu you didn t hear a singl word about tonight  tak a look ', 'i cal her today to tel her how proud i am of her and to mak sur she know what al wom should  we deserv to be heard ', 'a story appear today about someth that hap in       i was dismay when it occur  but was heart the young wom cam forward  was heard  and had her concern tak sery and address ', 'thank you  for yo tireless advoc on behalf of wom and girl  and for yo grac und press ov thes last    year  and thank you to for al you ve don and continu to do to adv reproduc right  onward ', 'for thos of you who don t know she s a suprem tal young wom with a ter podcast  she is also cour  amp  grac man a cant diagnos  amin  send you good vib post surgery  amp  shar yo inspir thread ', 'in       the wom s march was a beacon of hop and defy  in       it is a testa to the pow and resy of wom everywh  let s show that sam pow in the vot boo thi year ', 'rt rep  john lew  benny thompson to attend grand celebr of mississipp civil right muse', 'i m so heart by al of you  onward ', 'thes word from dr  king also com to mind today ', 'beauty said  an import mess today and every day ', 'the annivers of the devast earthquak   year ago is a day to rememb the tragedy  hon the resy peopl of hait   amp  affirm americ s commit to help our neighb  instead  we re subject to trump s ign  rac view of anyon who doesn t look lik him ', 'nant has a record of beat the od   from dc to ca   wher she help gov brown do a fantast job  al who know her ar send strength  amp  lov as she fac thi latest challeng  onward my friend   h', 'rt two week ago a    year old soldy rac rep into a burn bronx apart build  sav four peopl bef he ', "rt today  the amaz the destruct of hil clinton is out in paperback  we'r celebr with thi exc ", 'famy across americ had to start      worry that their kid wouldn t hav heal car  fail to act now show the tru fac of republ  amp  their don driv im agend  you control the sen agend enough is enough ', 'tim to bring chip to the sen flo as prom  thi alleg extend until march doesn t cut it as stat freez enrol  amp  send out let warn that cov wil end  thi is fright to par  amp  wreak havoc for stat ', 'the ir peopl  espec the young  ar protest for the freedom and fut they deserv  i hop their govern respond peac and support their hop ', 'rt retweet if you agr it s tot crazy to suggest that the fbi   hav help sink hil s campaign by rev that she ', 'rt must read  form clinton deputy  how rex tillerson can right the stat depart ', 'thank you to everyon who has don to onward togeth in our first year    we re on abl to support thes gre group becaus of you  let s do ev mor in       onward ', "along with and i know thes group wil continu to do the incred work of mak our democr stronger in       and i'm proud to be on their team ", 'and with the help of    afr am candid hav been elect to loc  stat  and fed off sint august      ', 'mor than     stud and lead met last mon at the pow summit to shar resourc and tool for nat advoc ', 'the team at is work to elect mor latino candid at every level of govern ', 'at young peopl ar us loc org to address econom ineq  attack on vot right  and mor ', "    brand new civ lead and act attend the very first    and they're on track to train       peopl by the end of      ", 'the org at work with partn on the ground to get autom vot reg on the ballot in nevad ', 'onward togeth is end      by support six mor incred org fight to protect vot right and to mak it easy for young  divers candid to get on the ballot and get elect ', 'rt what hap by was nam a best book of the year by     the new york tim    the washington post    npr ', 'someth produc to do with yo out today ', 'i guest edit thi mon s issu of it was a wond expery  with lot of ter contribut from peopl i lov  amp  respect  read thi let that my daught wrot to her childr about why she s stil optim about the fut  cc ', 'a littl girl pow in pasaden   ', 'som photo from the book tour  i met so many incred peopl  includ som littl hero   amp  the occas superhero   ', 'return hom aft the fin week of the book tour for       thi was the last cop of what hap i sign at our fin sign in seattl  what a ter journey thi has been so far  thank you to al who cam out  amp  told me yo story ', 'thank you to the  amp  for a ter discuss  a wond group of young peopl   amp  a tru inspir day ', 'today is the fin day to sign up for      heal cov  go to and get cov ', 'think of you  and al the sandy hook famy thi week  thank you for yo cour ', 'ye  elect mat ', "tonight  alabam vot elect a sen who'll mak them proud  and if democr can win in alabam  we can    and must    compet everywh  onward ", 'may ed lee s dea is a terr loss for the peopl of san francisco  he was a good friend  amp  a voc advoc for the city he serv  amp  lov  my thought ar with his famy ', "if you don't liv in an are that could be affect  read thi to see how you can help thos who do ", "think about al affect  amp  thos who could be affect by the californ wildfir  if you liv near a fir  mak sur you're prep   amp  stay saf ", 'i m going to keep tweet about thi  and speak out every chant i get  until it is fix ', 'so in thes sur tim  let s ral togeth  amp  tak act  cal yo hous and sen memb at              and tel them to tak car of kid now and protect our seny  the poor  amp  vuln from fut attack  tel them that it s the very least they can do ', 'how is it that in the middl of divid up      tril doll between corp  amp  the ultr wealthy  republ can t find the tim  amp  money to tak car of childr  thes ar pervers pri  congress nee to pass chip now  as they hav every year sint the     s ', 'it get wors  dur the campaign i warn that the rs would com aft soc sec  medic   amp  medicaid  amp  now they ar  imagin buy a rolex  amp  pay for it with money you sav to tak car of yo kid  that s what congress is doing w  yo tax doll ', 'meanwhil  sen republ rush to pass so cal tax reform   a giveaway for thos who least nee it ', "the children's heal ins program  which provid heal car for   mil kid  amp  has been reauth on a bipart bas every year for almost   decad  is hang in limbo becaus congress let it expir ov   month ago ", 'ther s a lot to be frust by right now  to say the least  her s someth that we should be abl to fix ', 'dr  king was    when the montgomery bus boycot beg  he start smal  ral oth who believ their effort mat  press on through challeng and doubt to chang our world for the bet  a perm inspir for the rest of us to keep push toward just ', 'al across americ peopl chos to get involv  get eng and stand up  each of us can mak a diff  and al of us ought to try  so go keep chang the world in      ', 'ten year old jahkil jackson is on a miss to help homeless peopl in chicago  he cre kit ful of sock  toiletry  and food for thos in nee  just thi week  jahkil reach his goal to giv away        bless bag   that s a story from      ', 'chris long gav his paycheck from the first six gam of the nfl season to fund scholarships in charlottesvil  va  he want to do mor  so he decid to giv away an entir season s sal  that s a story from      ', 'kat creech  a wed plan in houston  turn a postpon wed into a volunt opportun for hur harvey victim  thirty wed guest becam an org of hundr of volunt  that s a story from      ', "as we count down to the new year  we get to reflect and prep for what s ahead  for al the bad new that seem to domin our collect conscy  ther ar countless story from thi year that remind us what's best about americ ", 'rt i am my broth s keep  watch our new psa with  amp  then tak act to s ', 'on behalf of the obam famy  merry christmas  we wish you joy and peac thi holiday season ', "there's no bet tim than the holiday season to reach out and giv back to our commun  gre to hear from young peopl at the boy  amp  girl club in dc today ", 'happy hanukkah  everybody  from the obam famy to yo  chag sameach ', "just got off a cal to thank folk who ar work hard to help mor am across the country sign up for heal cov  but it's up to al of us to help spread the word  sign up through thi friday at", 'rt watch  we host a town hal in new delh with and young lead about how to driv chang and mak an im ', 'michel and i ar delight to congrat print harry and megh markl on their eng  we wish you a lifetim of joy and happy togeth ', 'from the obam famy to yo  we wish you a happy thanksg ful of joy and gratitud ', 'me  joe  about halfway through the speech  i m gonn wish you a happy bir   bid  it s my birthday  me  joe  happy birthday to my broth and the best vic presid anybody could hav ', 'rt today  we hon thos who hav hon our country with it highest form of serv ', "thi is what hap when the peopl vot  congr and   and congrat to al the vict in stat legisl  county and mayors' rac  every off in a democr count ", 'every elect mat   thos who show up determin our fut  go vot tomorrow ', 'may god also grant al of us the wisdom to ask what concret step we can tak to reduc the viol and weaponry in our midst ', 'we griev with al the famy in sutherland springs harm by thi act of hat  and we ll stand with the surv as they recov   ', 'start today  you can sign up for      heal cov  head on ov to and find a plan that meet yo nee ', "michel and i ar think of the victim of today's attack in nyc and everyon who keep us saf  new york ar as tough as they com ", 'hello  thrilled to host civ lead in chicago from al ov the world  follow along at', 'i ll let you and handl the sing  and we ll handl the don  ther s stil tim to giv ', 'tonight the ex presid ar get togeth in texa to support al our fellow am rebuild from thi year s hur  join us ', "i'm grat to for his lifetim of serv to our country  congrat  john  on receiv thi year's liberty med ", 'michel  amp  i ar pray for the victim in las vega  our thought ar with their famy  amp  everyon end anoth senseless tragedy ', 'proud to che on team us at the invict gam today with my friend joe  you repres the best of our country ', "we'r expand our effort to help puerto rico  amp  the usv  wher our fellow am nee us right now  join us at", 'prosecut  soldy  famy man  cit  beau mad us want to be bet  what a leg to leav  what a testa to', 'rt presid address start at       pm  tun in her ', 'think about our neighb in mexico and al our mex am friend tonight  cuidens mucho y un fuert abrazo par todo ', 'cod is import   and fun  thank for yo work to mak sur every kid can compet in a high tech  glob econom ', "michel and i want the to inspir and empow peopl to chang the world  here's how we'r get start thi fal ", 'we rememb everyon we lost on      and hon al who defend our country and our id  no act of ter wil ev chang who we ar ', 'rt across the u s   am hav answ the cal to help with hur recovery  pray for al florid ', 'proud of thes mckinley tech stud inspir young mind that mak me hop about our fut ', 'am alway answ the cal ', 'to target hop young strivers who grew up her is wrong  becaus they ve don noth wrong  my stat ', "thank you to al the first respond and peopl help each oth out  that's what we do as am  here's on way you can help now ", 'michel and i ar think of the victim and their famy in barcelon  am wil alway stand with our span friend  un abrazo ', '    for lov com mor nat to the hum heart than it opposit     nelson mandel', ' peopl must learn to hat  and if they can learn to hat  they can be taught to lov    ', ' no on is born hat anoth person becaus of the col of his skin or his background or his relig    ', "john mccain is an am hero  amp  on of the bravest fight i've ev known  cant doesn't know what it's up against  giv it hel  john ", "heal car has alway been about someth big than polit  it's about the charact of our country ", "of al that i've don in my lif  i'm most proud to be sash and malia's dad  to al thos lucky enough to be a dad  happy father's day ", 'on thi nat gun viol aw day  let yo voic be heard and show yo commit to reduc gun viol ', 'forev grat for the serv and sacr of al who fought to protect our freedom and defend thi country we lov ', 'good to see my friend print harry in london to discuss the work of our found  amp  off condol to victim of the manchest attack ', 'jerry seinfeld hold a baby pug at the grammy wil mak yo week i m just her becaus my alb   stand up comedy to mak lov to  was nomin ', '  wow  get som diff mat ', 'thi piec brought me clos to heav  thank alex  saw doc pom at the comedy club so much ', 'rt let of recommend  rodney dangerfield', 'trudeau turn to seinfeld tact to tam town hal heckl try throwing pap towel into the crowd ', 'don t miss my favorit show about a man and his mou ', 'jerry seinfeld at israel s ramon airbas with the israel air forc bomb  world of diff between them and me ', 'rt gre new  com in car get coff just went on netflix  you can see chris rock  amp  oth pal of jerry hav ', 'jerry seinfeld spot at tel av s best falafel shop  just nee someth to hold me ov until my din falafel ', "rt ar you a fan of com  car  coff  try not to freak out  but we'v got som new  season     ar now streaming  ", 'jerry seinfeld convint hugh jackm to retir as wolverin can t wait for thi post  com soon  jerry seinfeld as wolverin ', ' bee movy    celebr    year and ov   mil   thi is kind of weird   com mad ', 'rt nev thought i would be ment in the sam tweet as  ', 'ok  i ll  friend  you ', 'i m pick bridget everet s new sitcom as my favorit new  adult  comedy i ve seen thi year  so new  diff and funny  don t miss   lov you mor  wednesday night on amazon prim ', 'rt tough crowd  tak on', 'rt ther  we said it ', "as i've said many tim  if you don't get jerry lew  you don t understand comedy  spend an afternoon with   ", 'me and my man of three decad  enjoy melbourn ', 'ted l  nant read his let liv  includ shout out  la class  op jun   ', 'hil ', "debut of podcast  we cov automot psychos  hitler's     r  bonhom   amp  the hug thing ", "thank again to for yesterday's fath lunch  her s an interview i did with them ", "alright  i'm her in worcest for my show tonight and nobody her can ev agr on how the hel you say it  not giv up    ", "i cannot understand the spel and pronunt of worcest  mass  so i'm com in for   show saturday night to fig it out ", "i'm return to montreal's the first tim in ov    year with on      ", "there's a beauty new stag i lov it  thank you   let's do it again tonight   p ", "going to do a drop in set in about an hour   av   st  if you're in the neighb  ", "grey swe solid for my wife's new book   food swing   out today everywh  amp  already bestsel ", 'a smal but very import plac in my lif can now be an import but stil smal plac in yo lif ', 'meet me on tour     front row tix to my nyc show  bid on  amp  process wil benefit', 'thi plac ', "going to work out som stuff tonight in burbank  if you want to see what that's lik  ", '     mor class ', 'go see thi weekend i was just ther tonight  he destroy me  unbeliev funny ', 'new  com in car get coff  bob einstein is back  enjoy a second cup ', 'new  com in car get coff  christoph waltz  an eleg refin man  join us at ihop ', "pretty obvy the bas structure of my show was bas on the mtm show  hub of the wheel  let's say  but i ad her  perfect ", 'new  com in car get coff lew black  black s lif mat ', 'new com car get coff  cedr the entertain  no affy with cedr the regul person ', 'new episod today  com in car get coff norm macdonald  thi comedy standard is the norm ', 'season   premy   krist wiig  la  noth to do  two plac to get it don  cream ', "i do lov al the festiv nonsens every year  holiday ar nic but som sil was good to ad  thank again to dan o'keefe ", 'new singl shot   i want my old tv   the re memb on club ', 'lov thi and jerry l  so much  the ess of every com on display  wish thi was min ', 'new singl shot  is thi  a bit     men work  our motto  saf is numb    funny is numb   ', 'new singl shot lov thi car hat thi car  it s not wher you re going  it s if you can ev get ther ', 'new singl shot  coff rebel   com in car not drink coff ', 'rt i just earn my phd in new york city hist  and i found out i speak dutch from watch with', "rt you must watch 's on netflix  it's inspir and inspir  real extraordin work from cq ", 'ready for the weekend  ', 'tak a dant break at my thank and met towley for teach me the ', 'bts of the photoshoot with', 'today is holocaust remembr day  a day to hon the holocaust victim  may we nev forget ', 'thank you so much and for the beauty photo with the most amaz and tal company ', '  ', 'you ar wond wom  and of cours i wil    ', 'what a wond tim at the pga award last night ', 'unit we stand ', ' ', 'girl gon wild    when patty and i tak charad sery  ', '', 'tim to relax   ', 'i just hav to say thank you so so much to the team of the for hon me with the award last night  and also a hug thank you to the crit for hon as best act film ', '       ', 'thank you to the crit for recogn wond wom as best act movy  ', 'wow  i just watch thi whol video and i m in tear  so inspir and  ', 'thank you for the wond night and for hon patty and i with the spotlight award ', 'you know i lov a bold red lip  right  wel  i m about to mak it off  check out thi littl teas from', ' so i want al the girl watch her  now  to know that a new day is on the horizon    ', 'so happy to annount that i ve join the famy and am help launch the campaign  stay tun it s going to be beauty ', 'such an amaz night  surround by tal  inspir  and good peopl    ', 'thank you  ', 'wow  it s been an amaz year   excit for the tonight ', 'thank you   ', '', 'thank you palm springs intern film fest for the amaz night and hon me with the ris star award  big congr to al the oth hon of the night as wel ', 'i sign thi let of solid to stand with wom and men across every industry who hav expery sex harass  assault  or abus in say  enough is enough', "thank you      you've been incred       i'm ready for you     wish you al   happy  heal and luck   happy new year   ", 'i might hav been a littl mor excit than everyon els  ', 'look forward to the new year  ', 'happy holiday to al of you  may we alway be grat and smil  ', 'in such good company  thank you', 'feel very grat  thank you and for thi hon ', 'nev was i so happy on a monday  ready to start the holiday     ', 'thursday got me lik    ', 'thank you so much for the acknowledg and the kind word you wrot about us ', 'thank for ev lik thi so i can meet al of you      ', 'diff is spec  you re beauty keaton  insid and out ', 'no thank you  i couldn t hav don thi without you captain  lov you to the themyscir and back   ', 'hey ell  you re it   join the gam ', 'had such a wond tim at the gq men  wom  of the year party  thank you for hav me ', '  ', 'it was such an hon to be abl pres the first ev wond wom scholarship yesterday to thi wond wom  carl at the wom in entertain ev ', 'wow   year ago   and forev grat to be abl to play thi charact ', 'relax   ', 'thank you so much  hop al is wel   ', 'gisel', 'i real got the chil hear them   is thi real  ', 'so excit to meet you al  who is al com ']
    

We can also print the counts of each word in the vocabulary:


```python
def printCountsWord(vectorizer):
    vocab = vectorizer.get_feature_names()

    # Sum up the counts of each vocabulary word
    counts = np.sum(train_data_features, axis=0)

    # For each, print the vocabulary word and the number of times it 
    # appears in the training set
    for tag, count in zip(vocab[:10], counts[:10]): # taking just the first 10 words in the vocabulary for demonstration
        print(count, tag)

    print(len(vocab))
```

now we will use COuntVectorizor in order to create the bag of words, we will set the stop_words parameter to the stopword
in corpus in order to remove all of them for better classification.


```python
from nltk.corpus import stopwords # Import the stop word list

print("Creating the bag of words...\n")
from sklearn.feature_extraction.text import CountVectorizer

def create_bag_of_words(all_tweets):

    # Initialize the "CountVectorizer" object, which is scikit-learn's
    # bag of words tool.  
    vectorizer = CountVectorizer(analyzer = "word",   \
                                 tokenizer = None,    \
                                 preprocessor = None, \
                                 stop_words = stopwords.words("english") ,   \
                                 max_features = 1000) 

    # fit_transform() Convert a collection of text documents (reviews in our example) to a matrix of token counts.
    # This implementation produces a sparse representation.
    # The input to fit_transform should be a list of strings.
    train_data_features = vectorizer.fit_transform(all_tweets)
    ###train_data_features = vectorizer.fit_transform(train['review'])
    
    # Numpy arrays are easy to work with, so convert the result to an 
    # array
    return train_data_features.toarray()
    
train_data_features = create_bag_of_words(alltweets_text)
```

    Creating the bag of words...
    
    

Now we will divide the input into train and test


```python
#split to train & test
msk = np.random.rand(len(train_data_features)) < 0.8

train_x = train_data_features[msk]
test_x = train_data_features[~msk]
train_y = alltweets_names_as_df.loc[msk, 'Name']
test_y = alltweets_names_as_df.loc[~msk, 'Name']
```

# Random Forest

Now we will use the random forest algorithm, which uses many tree-based classifiers to make predictions.
We have the bag of words result for each sentence and we also have who wrote it for each one. 
Below we set the number of trees to 100 because we saw it has better performance.


```python
from sklearn.ensemble import RandomForestClassifier

# Initialize a Random Forest classifier with 100 trees
forest = RandomForestClassifier(n_estimators = 100) 

# Fit the forest to the training set, using the bag of words as 
# features and the sentiment labels as the response variable
forest = forest.fit( train_x, train_y )

# Evaluate accuracy best on the test set
random_forest = forest.score(test_x,test_y)

random_forest
```




    0.4807692307692308



# KNN

Now we will use the KNN algorithm,which make the disicion according to the majority of the neighbors of each new tweet.
The tuning parameters we did in this algorhitm is n_neighbors we set to be 13 and weights to be uniform.
We did this because we wanted the weight of each neighbor to be the same because we didnt want the distance from the neighbor to become factor. We chose n_neighbors to be 13 because we wanted to have enough neighbors so the prediction will be more accurate but not to many because it harms the accuracy when we take to many points.


```python
from sklearn.neighbors import KNeighborsClassifier

# Initialize a knn classifier with n_neighbors = 13, weights = 'uniform'
knn = KNeighborsClassifier(n_neighbors = 13, weights = 'uniform')

# Fit the knn to the training set, using the bag of words as 
# features and the sentiment labels as the response variable
knn = knn.fit( train_x, train_y )

# Evaluate accuracy best on the test set
k_near_neighbur = knn.score(test_x,test_y)

k_near_neighbur
```




    0.2692307692307692



# SVC

We used the SVM algorithm for predict the tweets owner.this is supervised learning models with associated learning algorithms that analyze data used for classification and regression analysis. Given a set of training examples, each marked as belonging to one or the other of two categories, an SVM training algorithm builds a model that assigns new examples to one category or the other, making it a non-probabilistic binary linear classifier In addition to performing linear classification, SVMs can efficiently perform a non-linear classification using what is called the kernel trick, implicitly mapping their inputs into high-dimensional feature spaces.This algorithm makes the disicion according to the location of the new tweet related to the hyperplane of the classifier. we use gamma to be 0.01 and c to be 100 and degree to be 3.  


```python
from sklearn import svm

# Initialize a svc classifier with gamma = 0.001, C = 100, degree = 3
svc = svm.SVC(gamma = 0.001, C = 100, degree = 3)

# Fit the svc to the training set, using the bag of words as 
# features and the sentiment labels as the response variable
svc = svc.fit( train_x, train_y )

# Evaluate accuracy best on the test set
support_vector = svc.score(test_x,test_y)

support_vector
```




    0.5384615384615384



# Logistic Regression

We used the logistic regression algorithm for predict the tweets owner.this is a regression model where the dependent variable (DV) is categorical. the logistic regression as no need for variables in the initialization process. this model performs well on data that as factorize pridection variable with not a lot of options. 


```python
from sklearn.linear_model import LogisticRegression

# Initialize a logistic regression classifier.
regresion = LogisticRegression()

# Fit the regression to the training set, using the bag of words as 
# features and the sentiment labels as the response variable
regresion = regresion.fit( train_x, train_y )

# Evaluate accuracy best on the test set
regression_logistic = regresion.score(test_x,test_y)

regression_logistic
```




    0.5769230769230769



# Naive Bayes

naive Bayes classifiers are a family of simple probabilistic classifiers based on applying Bayes' theorem with strong (naive) independence assumptions between the features.Naive Bayes classifiers are highly scalable, requiring a number of parameters linear in the number of variables (features/predictors) in a learning problem. Maximum-likelihood training can be done by evaluating a closed-form expression, which takes linear time, rather than by expensive iterative approximation as used for many other types of classifiers.the Naive Bayes as no need for variables in the initialization process.



```python
from sklearn.naive_bayes import GaussianNB

# Initialize a Naive Bayes classifier.
nb = GaussianNB()

# Fit the Naive Bayes to the training set, using the bag of words as 
# features and the sentiment labels as the response variable
nb = nb.fit( train_x, train_y )

# Evaluate accuracy best on the test set
naive_bayes = nb.score(test_x,test_y)

naive_bayes
```




    0.6346153846153846




```python
import matplotlib.pyplot as plt
import plotly.plotly as py

dictionary = plt.figure()

D = {u'Random_Forest':random_forest, u'KNN': k_near_neighbur, u'SVC':support_vector , u'Regression':regression_logistic, u'Naive_Bayes':naive_bayes}

plt.bar(range(len(D)), D.values(), align='center')
plt.xticks(range(len(D)), D.keys())

plt.show()
```


![png](output_35_0.png)


# Deep Learning


```python
!pip install keras==1.2
```

    Collecting keras==1.2
      Downloading Keras-1.2.0.tar.gz (167kB)
    Collecting theano (from keras==1.2)
      Downloading Theano-1.0.1.tar.gz (2.8MB)
    Requirement already satisfied: pyyaml in c:\anaconda\lib\site-packages (from keras==1.2)
    Requirement already satisfied: six in c:\anaconda\lib\site-packages (from keras==1.2)
    Requirement already satisfied: numpy>=1.9.1 in c:\anaconda\lib\site-packages (from theano->keras==1.2)
    Requirement already satisfied: scipy>=0.14 in c:\anaconda\lib\site-packages (from theano->keras==1.2)
    Building wheels for collected packages: keras, theano
      Running setup.py bdist_wheel for keras: started
      Running setup.py bdist_wheel for keras: finished with status 'done'
      Stored in directory: C:\Users\3AFA~1\AppData\Local\pip\Cache\wheels\f8\2c\8e\27d952128220b1acf6fcd5e692269c6e8d60744c15fd160c2c
      Running setup.py bdist_wheel for theano: started
      Running setup.py bdist_wheel for theano: finished with status 'done'
      Stored in directory: C:\Users\3AFA~1\AppData\Local\pip\Cache\wheels\46\a2\7d\b4cac381d5151daa9f9e0b3e4e4b65edaea6355ae296c97cf2
    Successfully built keras theano
    Installing collected packages: theano, keras
    Successfully installed keras-1.2.0 theano-1.0.1
    


```python
from keras import backend as K
K.set_image_dim_ordering('th')
```

    Using TensorFlow backend.
    C:\Anaconda\lib\site-packages\h5py\__init__.py:34: FutureWarning:
    
    Conversion of the second argument of issubdtype from `float` to `np.floating` is deprecated. In future, it will be treated as `np.float64 == np.dtype(float).type`.
    
    

We will take all trumps tweets and make them a single sequence.


```python
TWEET_DELIMITER = ' NEWTWEET '

def get_tweets_texts_joined(tweets):
    tweets_text = [tweet.full_text for tweet in tweets]
    return TWEET_DELIMITER.join(tweets_text)
    
trump_tweets = api.user_timeline(screen_name = "realDonaldTrump",count=500, tweet_mode = 'extended')
trump_tweets = get_tweets_texts_joined(trump_tweets)

hilary_tweets = api.user_timeline(screen_name = "HillaryClinton",count=500, tweet_mode = 'extended')
hilary_tweets = get_tweets_texts_joined(hilary_tweets)

obama_tweets = api.user_timeline(screen_name = "BarackObama",count=500, tweet_mode = 'extended')
obama_tweets = get_tweets_texts_joined(obama_tweets)

seinfeld_tweets = api.user_timeline(screen_name = "JerrySeinfeld",count=500, tweet_mode = 'extended')
seinfeld_tweets = get_tweets_texts_joined(seinfeld_tweets)

gadot_tweets = api.user_timeline(screen_name = "GalGadot",count=500, tweet_mode = 'extended')
gadot_tweets = get_tweets_texts_joined(gadot_tweets)
```

now we want to remove all hastag and tags (@)


```python
trump_tweets_updated = " ".join(filter(lambda x: '#' not in x and '@' not in x and 'http' not in x, trump_tweets.split()))
trump_tweets_updated

hilary_tweets_updated = " ".join(filter(lambda x: '#' not in x and '@' not in x and 'http' not in x, hilary_tweets.split()))
hilary_tweets_updated

obama_tweets_updated = " ".join(filter(lambda x: '#' not in x and '@' not in x and 'http' not in x, obama_tweets.split()))
obama_tweets_updated

seinfeld_tweets_updated = " ".join(filter(lambda x: '#' not in x and '@' not in x and 'http' not in x, seinfeld_tweets.split()))
seinfeld_tweets_updated

gadot_tweets_updated = " ".join(filter(lambda x: '#' not in x and '@' not in x and 'http' not in x, gadot_tweets.split()))
gadot_tweets_updated
```




    "Ready for the Weekend 😎 NEWTWEET Taking a dance break at my Thanks and Mette Towley for teaching me the🍋 NEWTWEET BTS of the photoshoot with NEWTWEET Today is Holocaust Remembrance Day. A day to honor the Holocaust Victims. May we never forget. NEWTWEET Thank you so much and for the beautiful photo with the most amazing and talented company! NEWTWEET 🌊🌊 NEWTWEET You ARE Wonder Woman. And of course i will! ❤️ NEWTWEET What a wonderful time at the PGA Awards last night. NEWTWEET United We Stand! NEWTWEET 🕶 NEWTWEET Girls gone wild..? When Patty and I take charades seriously 😅 NEWTWEET NEWTWEET Time to relax... NEWTWEET I just have to say thank you so so much to the team of the for honoring me with the award last night! And also a huge thank you to the critics for honoring as Best Action Film. NEWTWEET 🙅🏻\u200d♀️💃🏻 NEWTWEET Thank you to the Critics for recognizing Wonder Woman as best action movie!! NEWTWEET Wow! I just watched this whole video and I’m in tears. So inspirational and ! NEWTWEET Thank you for the wonderful night and for honoring Patty and I with the spotlight award. NEWTWEET You know I love a bold red lip, right? Well, I’m about to make it official. Check out this little teaser from NEWTWEET “So I want all the girls watching here, now, to know that a new day is on the horizon!” - NEWTWEET So happy to announce that I’ve joined the family and am helping launch the campaign. Stay tuned…it’s going to be beautiful! NEWTWEET Such an amazing night. Surrounded by talented, inspiring, and good people! ✌️ NEWTWEET Thank you ! NEWTWEET Wow. It’s been an amazing year!! Excited for the tonight! NEWTWEET Thank you ❤️ NEWTWEET NEWTWEET Thank you Palm Springs International Film Festival for the amazing night and honoring me with the Rising Star Award. Big congrats to all the other honorees of the night as well. NEWTWEET I signed this letter of solidarity to stand with women and men across every industry who have experienced sexual harassment, assault, or abuse in saying: enough is enough NEWTWEET Thank you 2017 you've been incredible. 2018 I'm ready for you.. 😏 wishing you all - happiness, health and luck!! Happy New Year!!! NEWTWEET I might have been a little more excited than everyone else 😂 NEWTWEET Looking forward to the New Year.. NEWTWEET Happy Holiday to all of you! May we always be grateful and smile 😊 NEWTWEET In such good company. Thank you NEWTWEET Feeling very grateful! Thank you and for this honor. NEWTWEET Never was I so happy on a Monday- ready to start the holidays 🤘🏼\U0001f91f🏼 NEWTWEET Thursday got me like.... NEWTWEET Thank you so much for the acknowledgment and the kind words you wrote about us! NEWTWEET Thankful for events like this so I can meet all of you! 🙅🏻\u200d♀ NEWTWEET Different is special. You’re beautiful Keaton. Inside and out. NEWTWEET No thank YOU! I couldn’t have done this without you Captain! Love you to the themyscira and back! 😘 NEWTWEET Hey Ella, you’re it! 💕Join the game! NEWTWEET Had such a wonderful time at the GQ Men (Woman) of the Year Party. Thank you for Having me! NEWTWEET 😱😃 NEWTWEET It was such an honor to be able present the first ever Wonder Woman scholarship yesterday to this WONDERful woman, Carla at the Women In Entertainment Event. NEWTWEET Wow 4 years ago?! And forever grateful to be able to play this character! NEWTWEET Relaxing... NEWTWEET Thank you so much. Hope all is well! 😘 NEWTWEET Gisele NEWTWEET I really got the chills hearing them.. is this real ? NEWTWEET So excited to meet you all! Who is all coming? NEWTWEET Thank you for this sit down with the very funny ! NEWTWEET Wow this is amazing! And I couldn’t ask for anyone else to receive this award with! thank you 🙏🏻❤️ NEWTWEET My partner in crime NEWTWEET Thank you E! News! So sweet! NEWTWEET NEWTWEET Thank you so much ! You’re the best! NEWTWEET Brooklynn, you are such a bright ray of light! So talented! I saw every bit of Wonder Woman in you when we met! 🙅🏻 NEWTWEET It was wonderful speaking with Willie Geist for NEWTWEET Thank you, thank you for your support! I’ve been reading your posts and seeing the photos in the theaters, and ticket stubs! You are the best fans! NEWTWEET RT Behind-the-scenes with GQ cover star NEWTWEET Bright and Early with the talking Justice League. Thanks for having me! NEWTWEET Such an honor!! Thank you so much ! NEWTWEET It might be unrealistic but my weekend goals! 😴 NEWTWEET This is incredible!!! So grateful for working w/ such amazing ppl &amp; for your amazing reviews 🙏🏻🙏🏻 NEWTWEET Last time we were all in London together we were filming Justice League. Can’t believe the time is here! NEWTWEET What is she is doing with my tiara?? NEWTWEET Press Tour Stop starts today in London! 🙅🏻💃🏻 NEWTWEET It was such an honor to be able to stand with these real life Wonder Woman. Thank you for letting me share this moment with you all! NEWTWEET Thank you for this amazing opportunity! NEWTWEET 😂😂 NEWTWEET 🙏🏻love your work! NEWTWEET The Tour is off to an amazing start! Thank you China for having us! 🙅🏻💃🏻🇨🇳 NEWTWEET China NEWTWEET 🙅🏻🙅🏻 NEWTWEET Very much noted! She’s a real life hero! Im sending her a huge hug of love, good energy and strength. NEWTWEET RT An amazing event for a very personal cause. My wonderful daughter Willow courageously leads our family as we walk to raise… NEWTWEET I’m such a big fan . You were so sweet today thanks for the awesome words! NEWTWEET NEWTWEET NEWTWEET It’s finally here! Tonight I’m hosting with musical guest Sam Smith ! Make sure you tune in! 💃🏻 Photos by Mary Ellen Matthews NEWTWEET RT Fun show tonight: is here, stop by, performs &amp; more! NEWTWEET Beyond excited to be hosting this Saturday with musical guest . Make sure you tune in! 💃🏻 NEWTWEET UK friends! You can get your hands on on Blu-ray on October 9! Patty wasn’t exaggerating when she said it was freezing cold.❄️ NEWTWEET RT NEW with &amp; more... NEWTWEET NEWTWEET No longer a secret, so excited to be hosting NEWTWEET Shana Tova to all of you! May this year be filled with joy , happiness, creation and love!! NEWTWEET Today is the day! is now available on Blu-ray™ I can't thank everyone enough for the love and support for this movie!🙅🏻 NEWTWEET What an amazing ride this has been! See the rest of the outtakes on the DVD extras 9/19. NEWTWEET 300 drones lit up the LA sky tonight to celebrate Power, Grace, Wisdom, and Wonder. Bring home on Digital Now and Blu-ray™ 9/19 NEWTWEET So well deserved my sister. Couldn't be happier for you. Paving the right way for so many to come NEWTWEET Thank you Rachel for the kind words about Wonder Woman! I'm a big fan of yours! Xo NEWTWEET There truly is a little Wonder in ALL of us! NEWTWEET Chris's nickname for me was giggle Gadot, &amp; I could communicate with no words. I enjoyed filming every second of the movie.💃🏻🙅🏻🎬 NEWTWEET Morgan and Meg are the perfect example of real life superheroes. Sending my strength and positive energy to both of you! NEWTWEET Just a taste of the fun we had while filming, more to come with the DVD's extras, on 9/19! We weren't always so serious on Themyscira. 😜💃🏽🙅🏻 NEWTWEET Looking amazing ladies! ❤️💃🏽💪🏻 NEWTWEET RT The Warrior Princess has arrived. Own on Digital Today! NEWTWEET It was an honor to present the Video of the Year award tonight at the ! Congrats to and all the other nominees. NEWTWEET RT NEWTWEET Wonder Woman was released in Japan! I hope you enjoy! ❤️ NEWTWEET Such an honor. Thank you ! NEWTWEET So true. NEWTWEET Wow! Just heard the news! Thank u to everyone who has shown their support to WW in theaters! What an amazing ride this has been! NEWTWEET Chasing Waterfalls 🌈🏞 NEWTWEET Sending all my love to Barcelona and those affected by this horrible tragedy NEWTWEET Thank you for the support of Wonder Woman last night, 3 awards- wow! We are blessed with fans like you! NEWTWEET Exciting news! is coming to Digital 8/29 and Blu-ray™ 9/19. NEWTWEET Another day at work. is raving &amp; is working &amp; I'm just trying to look cool. Art Direction: NEWTWEET I hope you enjoyed watching the NEWTWEET to standing on the blue carpet with our fearless leader, Patty Jenkins, for the premiere. NEWTWEET Amazing article – thank you 👏😘 NEWTWEET You can't save the world alone ✨❤️ from NEWTWEET from way for justice. Enjoy this sneak peak for now 🙏🏻 In theaters November 17. NEWTWEET Ready or not here we come...🤘🏼😏😆 benaffleck NEWTWEET These women are so inspirational! Love seeing them making an impact in the world of game design. NEWTWEET to last year at ❤️ Can’t wait to see you all this weekend 😉 NEWTWEET I love stumbling across photos from 💖💥 NEWTWEET It's the little things that make a difference 🙌 NEWTWEET Well said 🙏🏻💕 NEWTWEET This is incredible! 🙌 Thanks to all of you for making this possible 😀😘 NEWTWEET 🌸🌸 NEWTWEET Black and white glamour 😉💋 NEWTWEET unite 💥🙏🏻 from ✨ NEWTWEET When it's finally Friday... 🙌🏻😉 NEWTWEET Thanks to ALL of you for making a success. Wonder Woman has the best fans in the world ❤️ thx NEWTWEET Chocolate is always the answer 😉🙏 NEWTWEET NEWTWEET you're the best! Thank you for this.. love you and so much! 💋❤⭐️🤘🏼 NEWTWEET Wow 🙏 A huge thank u to all for making a success ❤️ thx for this piece NEWTWEET Getting thrown into Monday like... 😂 NEWTWEET 🖤🖤 NEWTWEET We had fun ❤️ Here is some more exclusive from ⚔️🙏🏻Enjoy ✨ NEWTWEET Kisses from my Lola 🐶😘 NEWTWEET NEWTWEET 💪🙏 NEWTWEET The smiling skies 😀 NEWTWEET Hope everyone is having a wonderful shared with family and friends 🙏🏻🙏🏻 NEWTWEET Hello, July 💋☀️ NEWTWEET Weekend got me like... 🙌🏻 NEWTWEET Always laughing with these two 😘😋 NEWTWEET NEWTWEET Another from 🙏 ❤️ NEWTWEET Relaxing and enjoying some scenery ❤️🌴 NEWTWEET 🖤 ⚔️ NEWTWEET So excited I finally get to share with you, Spain ✨⚔️🙏 disfrutar ❤️ NEWTWEET from 😘 NEWTWEET to this day ❤️ NEWTWEET Peek a boo 🙈🤗 NEWTWEET Happiness ☀️❤️ NEWTWEET 🖤 NEWTWEET Me + you = forever ❤️ NEWTWEET from Here’s some BTS footage from NEWTWEET To my other half , father of my children and love of my life . You are simply the… NEWTWEET Trying to hypnotize you... NEWTWEET Sleepless night , colic 3 months old baby and an early wake up by my 5 year old. Went to the… NEWTWEET Wow! Thx so much NEWTWEET It was always such a joy working with my family ❤️⚔️ We had a blast 💥🙌🏻 NEWTWEET And one more photo from ✨ NEWTWEET Wow! This is incredible 🙏👏 Bravo NEWTWEET Make sure to always eat your greens 💚 NEWTWEET This is awesome! 🙏 NEWTWEET Wow the last paragraph really gave me the chills. So true. So powerful. Gives me a huge drive to dive in and work on the next one..🙏🏻 NEWTWEET Pure joy working on bc I was surrounded by great people❤️ NEWTWEET Thank you for having me Grazia China 🇨🇳 NEWTWEET RT It's official! has become the most Tweeted movie of 2017. Congrats to and the cas… NEWTWEET Can’t thank you all enough for making 💥⚔🙏 NEWTWEET 💜💙🙏 thx for this cool piece 💋 NEWTWEET To my fans. Thank you all, I love you all so much 🙏🏻😘💋 תודה, धन्यवाद, Merci, Danke, 谢谢, Gracias, ありがと, Obrigado ❤️ NEWTWEET What was your favorite part of costume? ✨🙏 from NEWTWEET from 💋I thought she knew me 😩😜🍸 NEWTWEET Thanks so much my friend 😘🙏🏻 NEWTWEET Take that 😉💥✨ NEWTWEET 🙏🏻🙏🏻🙏🏻 NEWTWEET Wow. Thank you so much. Means a lot coming from you 🙏🏻 NEWTWEET 🙏🏻🍀😘 NEWTWEET Thank you so much James, so happy you enjoyed it. Can't wait for Aquaman!!! NEWTWEET So happy you liked it Josh 💋 NEWTWEET Wow thank you so much 🙏🏻 sending you a kiss back!! NEWTWEET WOW! This is so moving and inspiring! NEWTWEET LOL, thank you so much! c u soon 😉 NEWTWEET Looking good girl! NEWTWEET It looks so good when you do it 🙅🏻🙅🏻 NEWTWEET The cutest!😍 NEWTWEET I always knew you were a smart guy :) But I think its worth a fight . we should collide worlds😏 NEWTWEET Wow thank you sister. I'm a big fan of your work and I'm so happy you enjoyed it 🙏 NEWTWEET .Thank you so much for the love 🙏 Miss you big guy!! 😘 NEWTWEET Can't believe how much fun and a joy it was making this movie. Truly blessed to work with these amazing people. NEWTWEET Hello players and fans, I am so happy to be the spokesperson for League of Angels - Paradise Land available now for all of you to play! NEWTWEET TODAY is the day! 🙌 ✨ is officially in theaters everywhere. Much love to all 💥⚔️ NEWTWEET Its almost time ⚔️💥 So thrilled to share with you all the latest poster for 🖤✨ NEWTWEET Add a bit more Wonder to your text messages with the new stickers ✨💙 Stickers available for iOS and Andriod download now ❤️ NEWTWEET Now this is a super squad ✨😉🙌 Thx ❤️ in theaters tomorrow! NEWTWEET Thx for this feature ❤️ Im honored to work alongside someone who I admire. Love u, 💋💥 NEWTWEET Loved having you by my side ✨❤️😘 Muah 💋 NEWTWEET I'm so excited to share my cover with you all. Thx so much 💋❤️ 📸 Photo taken by NEWTWEET כשאמאבא שלי שלחו את זה הייתי בטוחה שעשו את זה במחשב ואז קיבלתי עוד תמונה מבת דודה שלי , חברה… NEWTWEET Thank you for having us Mexico City! Always great to come here . Gracias por la cálida… NEWTWEET RT has a very simple reason for wearing flats instead of heels to the premiere. NEWTWEET Live from the red carpet of the premiere in Mexico -"



Because tweets are not very long we would like to keep only the most frequent words,
so we will set the vocabulary length to 150 because for the other words we don’t have a lot of contextual examples, 
so we wouldn’t be able to learn how to use them correctly anyway.
We replace all words not included in our vocabulary by UNKNOWN_TOKEN.
We also want to learn which words tend start and end a sentence. To do this we prepend a special SENTENCE_START token, and append a special SENTENCE_END token to each sentence
in order for the new line ans the seperator  to be preserved, let's convert it to speciel tokens, otherwise it will be removed from the text with the rest of the punctuations.


```python
vocabulary_size = 600
unknown_token = "UNKNOWNTOKEN"
sentence_start_token = "SENTENCESTART"
sentence_end_token = "SENTENCEEND"
line_break= "NEWLINE"
separator= "SEPARATOR"
```

Let's convert these special characters into the mentioned tokens


```python
def replace_specific_symbols_and_clean(text):
    text = text.replace('...', '.')
    text = text.replace('....', '.')
    text = text.replace('\n',' '+ line_break + ' ')
    text = text.replace('--',' '+ separator + ' ')
    text = text.replace('.',' '+sentence_end_token +' '+ sentence_start_token+' ' )
    return clean_text(text)
```


```python
trump_tweets_cleaned = replace_specific_symbols_and_clean(trump_tweets_updated)
trump_tweets_cleaned

hilary_tweets_cleaned = replace_specific_symbols_and_clean(hilary_tweets_updated)
hilary_tweets_cleaned

obama_tweets_cleaned = replace_specific_symbols_and_clean(obama_tweets_updated)
obama_tweets_cleaned

seinfeld_tweets_cleaned = replace_specific_symbols_and_clean(seinfeld_tweets_updated)
seinfeld_tweets_cleaned

gadot_tweets_cleaned = replace_specific_symbols_and_clean(gadot_tweets_updated)
gadot_tweets_cleaned
```




    "Ready for the Weekend   NEWTWEET Taking a dance break at my Thanks and Mette Towley for teaching me the  NEWTWEET BTS of the photoshoot with NEWTWEET Today is Holocaust Remembrance Day SENTENCEEND SENTENCESTART A day to honor the Holocaust Victims SENTENCEEND SENTENCESTART May we never forget SENTENCEEND SENTENCESTART NEWTWEET Thank you so much and for the beautiful photo with the most amazing and talented company  NEWTWEET    NEWTWEET You ARE Wonder Woman SENTENCEEND SENTENCESTART And of course i will     NEWTWEET What a wonderful time at the PGA Awards last night SENTENCEEND SENTENCESTART NEWTWEET United We Stand  NEWTWEET   NEWTWEET Girls gone wild SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART   When Patty and I take charades seriously   NEWTWEET NEWTWEET Time to relax SENTENCEEND SENTENCESTART NEWTWEET I just have to say thank you so so much to the team of the for honoring me with the award last night  And also a huge thank you to the critics for honoring as Best Action Film SENTENCEEND SENTENCESTART NEWTWEET         NEWTWEET Thank you to the Critics for recognizing Wonder Woman as best action movie   NEWTWEET Wow  I just watched this whole video and I m in tears SENTENCEEND SENTENCESTART So inspirational and   NEWTWEET Thank you for the wonderful night and for honoring Patty and I with the spotlight award SENTENCEEND SENTENCESTART NEWTWEET You know I love a bold red lip  right  Well  I m about to make it official SENTENCEEND SENTENCESTART Check out this little teaser from NEWTWEET  So I want all the girls watching here  now  to know that a new day is on the horizon     NEWTWEET So happy to announce that I ve joined the family and am helping launch the campaign SENTENCEEND SENTENCESTART Stay tuned it s going to be beautiful  NEWTWEET Such an amazing night SENTENCEEND SENTENCESTART Surrounded by talented  inspiring  and good people     NEWTWEET Thank you   NEWTWEET Wow SENTENCEEND SENTENCESTART It s been an amazing year   Excited for the tonight  NEWTWEET Thank you    NEWTWEET NEWTWEET Thank you Palm Springs International Film Festival for the amazing night and honoring me with the Rising Star Award SENTENCEEND SENTENCESTART Big congrats to all the other honorees of the night as well SENTENCEEND SENTENCESTART NEWTWEET I signed this letter of solidarity to stand with women and men across every industry who have experienced sexual harassment  assault  or abuse in saying  enough is enough NEWTWEET Thank you      you've been incredible SENTENCEEND SENTENCESTART      I'm ready for you SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART   wishing you all   happiness  health and luck   Happy New Year    NEWTWEET I might have been a little more excited than everyone else   NEWTWEET Looking forward to the New Year SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART NEWTWEET Happy Holiday to all of you  May we always be grateful and smile   NEWTWEET In such good company SENTENCEEND SENTENCESTART Thank you NEWTWEET Feeling very grateful  Thank you and for this honor SENTENCEEND SENTENCESTART NEWTWEET Never was I so happy on a Monday  ready to start the holidays      NEWTWEET Thursday got me like SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART NEWTWEET Thank you so much for the acknowledgment and the kind words you wrote about us  NEWTWEET Thankful for events like this so I can meet all of you       NEWTWEET Different is special SENTENCEEND SENTENCESTART You re beautiful Keaton SENTENCEEND SENTENCESTART Inside and out SENTENCEEND SENTENCESTART NEWTWEET No thank YOU  I couldn t have done this without you Captain  Love you to the themyscira and back    NEWTWEET Hey Ella  you re it   Join the game  NEWTWEET Had such a wonderful time at the GQ Men  Woman  of the Year Party SENTENCEEND SENTENCESTART Thank you for Having me  NEWTWEET    NEWTWEET It was such an honor to be able present the first ever Wonder Woman scholarship yesterday to this WONDERful woman  Carla at the Women In Entertainment Event SENTENCEEND SENTENCESTART NEWTWEET Wow   years ago   And forever grateful to be able to play this character  NEWTWEET Relaxing SENTENCEEND SENTENCESTART NEWTWEET Thank you so much SENTENCEEND SENTENCESTART Hope all is well    NEWTWEET Gisele NEWTWEET I really got the chills hearing them SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART is this real   NEWTWEET So excited to meet you all  Who is all coming  NEWTWEET Thank you for this sit down with the very funny   NEWTWEET Wow this is amazing  And I couldn t ask for anyone else to receive this award with  thank you      NEWTWEET My partner in crime NEWTWEET Thank you E  News  So sweet  NEWTWEET NEWTWEET Thank you so much   You re the best  NEWTWEET Brooklynn  you are such a bright ray of light  So talented  I saw every bit of Wonder Woman in you when we met     NEWTWEET It was wonderful speaking with Willie Geist for NEWTWEET Thank you  thank you for your support  I ve been reading your posts and seeing the photos in the theaters  and ticket stubs  You are the best fans  NEWTWEET RT Behind the scenes with GQ cover star NEWTWEET Bright and Early with the talking Justice League SENTENCEEND SENTENCESTART Thanks for having me  NEWTWEET Such an honor   Thank you so much   NEWTWEET It might be unrealistic but my weekend goals    NEWTWEET This is incredible    So grateful for working w  such amazing ppl  amp  for your amazing reviews      NEWTWEET Last time we were all in London together we were filming Justice League SENTENCEEND SENTENCESTART Can t believe the time is here  NEWTWEET What is she is doing with my tiara   NEWTWEET Press Tour Stop starts today in London       NEWTWEET It was such an honor to be able to stand with these real life Wonder Woman SENTENCEEND SENTENCESTART Thank you for letting me share this moment with you all  NEWTWEET Thank you for this amazing opportunity  NEWTWEET    NEWTWEET   love your work  NEWTWEET The Tour is off to an amazing start  Thank you China for having us         NEWTWEET China NEWTWEET      NEWTWEET Very much noted  She s a real life hero  Im sending her a huge hug of love  good energy and strength SENTENCEEND SENTENCESTART NEWTWEET RT An amazing event for a very personal cause SENTENCEEND SENTENCESTART My wonderful daughter Willow courageously leads our family as we walk to raise  NEWTWEET I m such a big fan SENTENCEEND SENTENCESTART You were so sweet today thanks for the awesome words  NEWTWEET NEWTWEET NEWTWEET It s finally here  Tonight I m hosting with musical guest Sam Smith   Make sure you tune in     Photos by Mary Ellen Matthews NEWTWEET RT Fun show tonight  is here  stop by  performs  amp  more  NEWTWEET Beyond excited to be hosting this Saturday with musical guest SENTENCEEND SENTENCESTART Make sure you tune in     NEWTWEET UK friends  You can get your hands on on Blu ray on October    Patty wasn t exaggerating when she said it was freezing cold SENTENCEEND SENTENCESTART    NEWTWEET RT NEW with  amp  more SENTENCEEND SENTENCESTART NEWTWEET NEWTWEET No longer a secret  so excited to be hosting NEWTWEET Shana Tova to all of you  May this year be filled with joy   happiness  creation and love   NEWTWEET Today is the day  is now available on Blu ray  I can't thank everyone enough for the love and support for this movie    NEWTWEET What an amazing ride this has been  See the rest of the outtakes on the DVD extras      SENTENCEEND SENTENCESTART NEWTWEET     drones lit up the LA sky tonight to celebrate Power  Grace  Wisdom  and Wonder SENTENCEEND SENTENCESTART Bring home on Digital Now and Blu ray       NEWTWEET So well deserved my sister SENTENCEEND SENTENCESTART Couldn't be happier for you SENTENCEEND SENTENCESTART Paving the right way for so many to come NEWTWEET Thank you Rachel for the kind words about Wonder Woman  I'm a big fan of yours  Xo NEWTWEET There truly is a little Wonder in ALL of us  NEWTWEET Chris's nickname for me was giggle Gadot   amp  I could communicate with no words SENTENCEEND SENTENCESTART I enjoyed filming every second of the movie SENTENCEEND SENTENCESTART       NEWTWEET Morgan and Meg are the perfect example of real life superheroes SENTENCEEND SENTENCESTART Sending my strength and positive energy to both of you  NEWTWEET Just a taste of the fun we had while filming  more to come with the DVD's extras  on       We weren't always so serious on Themyscira SENTENCEEND SENTENCESTART       NEWTWEET Looking amazing ladies         NEWTWEET RT The Warrior Princess has arrived SENTENCEEND SENTENCESTART Own on Digital Today  NEWTWEET It was an honor to present the Video of the Year award tonight at the   Congrats to and all the other nominees SENTENCEEND SENTENCESTART NEWTWEET RT NEWTWEET Wonder Woman was released in Japan  I hope you enjoy     NEWTWEET Such an honor SENTENCEEND SENTENCESTART Thank you   NEWTWEET So true SENTENCEEND SENTENCESTART NEWTWEET Wow  Just heard the news  Thank u to everyone who has shown their support to WW in theaters  What an amazing ride this has been  NEWTWEET Chasing Waterfalls    NEWTWEET Sending all my love to Barcelona and those affected by this horrible tragedy NEWTWEET Thank you for the support of Wonder Woman last night    awards  wow  We are blessed with fans like you  NEWTWEET Exciting news  is coming to Digital      and Blu ray       SENTENCEEND SENTENCESTART NEWTWEET Another day at work SENTENCEEND SENTENCESTART is raving  amp  is working  amp  I'm just trying to look cool SENTENCEEND SENTENCESTART Art Direction  NEWTWEET I hope you enjoyed watching the NEWTWEET to standing on the blue carpet with our fearless leader  Patty Jenkins  for the premiere SENTENCEEND SENTENCESTART NEWTWEET Amazing article   thank you    NEWTWEET You can't save the world alone     from NEWTWEET from way for justice SENTENCEEND SENTENCESTART Enjoy this sneak peak for now    In theaters November    SENTENCEEND SENTENCESTART NEWTWEET Ready or not here we come SENTENCEEND SENTENCESTART      benaffleck NEWTWEET These women are so inspirational  Love seeing them making an impact in the world of game design SENTENCEEND SENTENCESTART NEWTWEET to last year at    Can t wait to see you all this weekend   NEWTWEET I love stumbling across photos from    NEWTWEET It's the little things that make a difference   NEWTWEET Well said     NEWTWEET This is incredible    Thanks to all of you for making this possible    NEWTWEET    NEWTWEET Black and white glamour    NEWTWEET unite     from   NEWTWEET When it's finally Friday SENTENCEEND SENTENCESTART     NEWTWEET Thanks to ALL of you for making a success SENTENCEEND SENTENCESTART Wonder Woman has the best fans in the world    thx NEWTWEET Chocolate is always the answer    NEWTWEET NEWTWEET you're the best  Thank you for this SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART love you and so much         NEWTWEET Wow   A huge thank u to all for making a success    thx for this piece NEWTWEET Getting thrown into Monday like SENTENCEEND SENTENCESTART   NEWTWEET    NEWTWEET We had fun    Here is some more exclusive from     Enjoy   NEWTWEET Kisses from my Lola    NEWTWEET NEWTWEET    NEWTWEET The smiling skies   NEWTWEET Hope everyone is having a wonderful shared with family and friends      NEWTWEET Hello  July     NEWTWEET Weekend got me like SENTENCEEND SENTENCESTART    NEWTWEET Always laughing with these two    NEWTWEET NEWTWEET Another from      NEWTWEET Relaxing and enjoying some scenery     NEWTWEET      NEWTWEET So excited I finally get to share with you  Spain      disfrutar    NEWTWEET from   NEWTWEET to this day    NEWTWEET Peek a boo    NEWTWEET Happiness      NEWTWEET   NEWTWEET Me   you   forever    NEWTWEET from Here s some BTS footage from NEWTWEET To my other half   father of my children and love of my life SENTENCEEND SENTENCESTART You are simply the  NEWTWEET Trying to hypnotize you SENTENCEEND SENTENCESTART NEWTWEET Sleepless night   colic   months old baby and an early wake up by my   year old SENTENCEEND SENTENCESTART Went to the  NEWTWEET Wow  Thx so much NEWTWEET It was always such a joy working with my family      We had a blast     NEWTWEET And one more photo from   NEWTWEET Wow  This is incredible    Bravo NEWTWEET Make sure to always eat your greens   NEWTWEET This is awesome    NEWTWEET Wow the last paragraph really gave me the chills SENTENCEEND SENTENCESTART So true SENTENCEEND SENTENCESTART So powerful SENTENCEEND SENTENCESTART Gives me a huge drive to dive in and work on the next one SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART    NEWTWEET Pure joy working on bc I was surrounded by great people   NEWTWEET Thank you for having me Grazia China    NEWTWEET RT It's official  has become the most Tweeted movie of      SENTENCEEND SENTENCESTART Congrats to and the cas  NEWTWEET Can t thank you all enough for making     NEWTWEET     thx for this cool piece   NEWTWEET To my fans SENTENCEEND SENTENCESTART Thank you all  I love you all so much                     Merci  Danke      Gracias        Obrigado    NEWTWEET What was your favorite part of costume     from NEWTWEET from  I thought she knew me     NEWTWEET Thanks so much my friend     NEWTWEET Take that     NEWTWEET        NEWTWEET Wow SENTENCEEND SENTENCESTART Thank you so much SENTENCEEND SENTENCESTART Means a lot coming from you    NEWTWEET      NEWTWEET Thank you so much James  so happy you enjoyed it SENTENCEEND SENTENCESTART Can't wait for Aquaman    NEWTWEET So happy you liked it Josh   NEWTWEET Wow thank you so much    sending you a kiss back   NEWTWEET WOW  This is so moving and inspiring  NEWTWEET LOL  thank you so much  c u soon   NEWTWEET Looking good girl  NEWTWEET It looks so good when you do it      NEWTWEET The cutest   NEWTWEET I always knew you were a smart guy    But I think its worth a fight SENTENCEEND SENTENCESTART we should collide worlds  NEWTWEET Wow thank you sister SENTENCEEND SENTENCESTART I'm a big fan of your work and I'm so happy you enjoyed it   NEWTWEET SENTENCEEND SENTENCESTART Thank you so much for the love   Miss you big guy     NEWTWEET Can't believe how much fun and a joy it was making this movie SENTENCEEND SENTENCESTART Truly blessed to work with these amazing people SENTENCEEND SENTENCESTART NEWTWEET Hello players and fans  I am so happy to be the spokesperson for League of Angels   Paradise Land available now for all of you to play  NEWTWEET TODAY is the day      is officially in theaters everywhere SENTENCEEND SENTENCESTART Much love to all     NEWTWEET Its almost time     So thrilled to share with you all the latest poster for    NEWTWEET Add a bit more Wonder to your text messages with the new stickers    Stickers available for iOS and Andriod download now    NEWTWEET Now this is a super squad     Thx    in theaters tomorrow  NEWTWEET Thx for this feature    Im honored to work alongside someone who I admire SENTENCEEND SENTENCESTART Love u     NEWTWEET Loved having you by my side      Muah   NEWTWEET I'm so excited to share my cover with you all SENTENCEEND SENTENCESTART Thx so much       Photo taken by NEWTWEET                                                                                               NEWTWEET Thank you for having us Mexico City  Always great to come here SENTENCEEND SENTENCESTART Gracias por la c lida  NEWTWEET RT has a very simple reason for wearing flats instead of heels to the premiere SENTENCEEND SENTENCESTART NEWTWEET Live from the red carpet of the premiere in Mexico  "



text_to_word_sequence splits a sentence into a list of words


```python
from keras.preprocessing.text import text_to_word_sequence
trump_text_splitted = text_to_word_sequence(trump_tweets_cleaned, lower=False, split=" ") #using only 10000 first words
trump_text_splitted[0:10]

hilary_text_splitted = text_to_word_sequence(hilary_tweets_cleaned, lower=False, split=" ") #using only 10000 first words
hilary_text_splitted[0:10]

obama_text_splitted = text_to_word_sequence(obama_tweets_cleaned, lower=False, split=" ") #using only 10000 first words
obama_text_splitted[0:10]

seinfeld_text_splitted = text_to_word_sequence(seinfeld_tweets_cleaned, lower=False, split=" ") #using only 10000 first words
seinfeld_text_splitted[0:10]

gadot_text_splitted = text_to_word_sequence(gadot_tweets_cleaned, lower=False, split=" ") #using only 10000 first words
gadot_text_splitted[0:10]
```




    ['Ready',
     'for',
     'the',
     'Weekend',
     'NEWTWEET',
     'Taking',
     'a',
     'dance',
     'break',
     'at']



Tokenizer is a class for vectorizing texts, we would like to convert each word into a list of word indexes. it considers the vocabulary size for indexing most frequent words, otherwise replace them by unknown-token index.


```python
from keras.preprocessing.text import Tokenizer
trump_tokenizer = Tokenizer(nb_words=150,char_level=False)
trump_tokenizer.fit_on_texts(trump_text_splitted)

hilary_tokenizer = Tokenizer(nb_words=150,char_level=False)
hilary_tokenizer.fit_on_texts(hilary_text_splitted)

obama_tokenizer = Tokenizer(nb_words=150,char_level=False)
obama_tokenizer.fit_on_texts(obama_text_splitted)

seinfeld_tokenizer = Tokenizer(nb_words=150,char_level=False)
seinfeld_tokenizer.fit_on_texts(seinfeld_text_splitted)

gadot_tokenizer = Tokenizer(nb_words=150,char_level=False)
gadot_tokenizer.fit_on_texts(gadot_text_splitted)
```

Each word will become a vector, and the input will be a matrix, with each row representing a word. 
texts_to_matrix performs this conversion for us when setting the mode parameter to 'binary'.


```python
trump_text_mtx = trump_tokenizer.texts_to_matrix(trump_text_splitted, mode='binary')

hilary_text_mtx = hilary_tokenizer.texts_to_matrix(hilary_text_splitted, mode='binary')

obama_text_mtx = obama_tokenizer.texts_to_matrix(obama_text_splitted, mode='binary')

sienfeld_text_mtx = seinfeld_tokenizer.texts_to_matrix(seinfeld_text_splitted, mode='binary')

gadot_text_mtx = gadot_tokenizer.texts_to_matrix(gadot_text_splitted, mode='binary')
```

We would like to predict the next word, so output is just the input matrix shifted by one row It is that simple to do it:


```python
trump_input = trump_text_mtx[:-1]
trump_output = trump_text_mtx[1:]

hilary_input = hilary_text_mtx[:-1]
hilary_output = hilary_text_mtx[1:]

obama_input = obama_text_mtx[:-1]
obama_output = obama_text_mtx[1:]

seinfeld_input = sienfeld_text_mtx[:-1]
seinfeld_output = sienfeld_text_mtx[1:]

gadot_input = gadot_text_mtx[:-1]
gadot_output = gadot_text_mtx[1:]
```

## Recurrent Neural Network

Let's use SimpleRNN to add a fully-connected RNN where the output is to be fed back to input.

lets create the vocabulary


```python
import pandas as pd
import numpy as np
trump_vocab = pd.DataFrame({'word':trump_text_splitted,'code':np.argmax(trump_text_mtx,axis=1)})

hilary_vocab = pd.DataFrame({'word':hilary_text_splitted,'code':np.argmax(hilary_text_mtx,axis=1)})

obama_vocab = pd.DataFrame({'word':obama_text_splitted,'code':np.argmax(obama_text_mtx,axis=1)})

seinfeld_vocab = pd.DataFrame({'word':seinfeld_text_splitted,'code':np.argmax(sienfeld_text_mtx,axis=1)})

gadot_vocab = pd.DataFrame({'word':gadot_text_splitted,'code':np.argmax(gadot_text_mtx,axis=1)})
```


```python
from keras.models import Sequential
from keras.layers.core import Dense, Activation, Flatten
from keras.layers.wrappers import TimeDistributed
from keras.layers.embeddings import Embedding
from keras.layers.recurrent import LSTM
from keras.layers.embeddings import Embedding
from keras.layers.recurrent import SimpleRNN
```


```python
trump_model = Sequential()

hilary_model = Sequential()

obama_model = Sequential()

seinfeld_model = Sequential()

gadot_model = Sequential()
```


```python
trump_model.add(Embedding(input_dim=trump_input.shape[1],output_dim= 42, input_length=trump_input.shape[1]))

hilary_model.add(Embedding(input_dim=hilary_input.shape[1],output_dim= 42, input_length=hilary_input.shape[1]))

obama_model.add(Embedding(input_dim=obama_input.shape[1],output_dim= 42, input_length=obama_input.shape[1]))

seinfeld_model.add(Embedding(input_dim=seinfeld_input.shape[1],output_dim= 42, input_length=seinfeld_input.shape[1]))

gadot_model.add(Embedding(input_dim=gadot_input.shape[1],output_dim= 42, input_length=gadot_input.shape[1]))
```


```python
trump_model.add(Flatten())
trump_model.add(Dense(trump_output.shape[1], activation='sigmoid'))

hilary_model.add(Flatten())
hilary_model.add(Dense(hilary_output.shape[1], activation='sigmoid'))

obama_model.add(Flatten())
obama_model.add(Dense(obama_output.shape[1], activation='sigmoid'))

seinfeld_model.add(Flatten())
seinfeld_model.add(Dense(seinfeld_output.shape[1], activation='sigmoid'))

gadot_model.add(Flatten())
gadot_model.add(Dense(gadot_output.shape[1], activation='sigmoid'))
```


```python
trump_model.summary(line_length=100)

hilary_model.summary(line_length=100)

obama_model.summary(line_length=100)

seinfeld_model.summary(line_length=100)

gadot_model.summary(line_length=100)
```

    ____________________________________________________________________________________________________
    Layer (type)                     Output Shape          Param #     Connected to                     
    ====================================================================================================
    embedding_1 (Embedding)          (None, 150, 42)       6300        embedding_input_1[0][0]          
    ____________________________________________________________________________________________________
    flatten_1 (Flatten)              (None, 6300)          0           embedding_1[0][0]                
    ____________________________________________________________________________________________________
    dense_1 (Dense)                  (None, 150)           945150      flatten_1[0][0]                  
    ====================================================================================================
    Total params: 951,450
    Trainable params: 951,450
    Non-trainable params: 0
    ____________________________________________________________________________________________________
    ____________________________________________________________________________________________________
    Layer (type)                     Output Shape          Param #     Connected to                     
    ====================================================================================================
    embedding_2 (Embedding)          (None, 150, 42)       6300        embedding_input_2[0][0]          
    ____________________________________________________________________________________________________
    flatten_2 (Flatten)              (None, 6300)          0           embedding_2[0][0]                
    ____________________________________________________________________________________________________
    dense_2 (Dense)                  (None, 150)           945150      flatten_2[0][0]                  
    ====================================================================================================
    Total params: 951,450
    Trainable params: 951,450
    Non-trainable params: 0
    ____________________________________________________________________________________________________
    ____________________________________________________________________________________________________
    Layer (type)                     Output Shape          Param #     Connected to                     
    ====================================================================================================
    embedding_3 (Embedding)          (None, 150, 42)       6300        embedding_input_3[0][0]          
    ____________________________________________________________________________________________________
    flatten_3 (Flatten)              (None, 6300)          0           embedding_3[0][0]                
    ____________________________________________________________________________________________________
    dense_3 (Dense)                  (None, 150)           945150      flatten_3[0][0]                  
    ====================================================================================================
    Total params: 951,450
    Trainable params: 951,450
    Non-trainable params: 0
    ____________________________________________________________________________________________________
    ____________________________________________________________________________________________________
    Layer (type)                     Output Shape          Param #     Connected to                     
    ====================================================================================================
    embedding_4 (Embedding)          (None, 150, 42)       6300        embedding_input_4[0][0]          
    ____________________________________________________________________________________________________
    flatten_4 (Flatten)              (None, 6300)          0           embedding_4[0][0]                
    ____________________________________________________________________________________________________
    dense_4 (Dense)                  (None, 150)           945150      flatten_4[0][0]                  
    ====================================================================================================
    Total params: 951,450
    Trainable params: 951,450
    Non-trainable params: 0
    ____________________________________________________________________________________________________
    ____________________________________________________________________________________________________
    Layer (type)                     Output Shape          Param #     Connected to                     
    ====================================================================================================
    embedding_5 (Embedding)          (None, 150, 42)       6300        embedding_input_5[0][0]          
    ____________________________________________________________________________________________________
    flatten_5 (Flatten)              (None, 6300)          0           embedding_5[0][0]                
    ____________________________________________________________________________________________________
    dense_5 (Dense)                  (None, 150)           945150      flatten_5[0][0]                  
    ====================================================================================================
    Total params: 951,450
    Trainable params: 951,450
    Non-trainable params: 0
    ____________________________________________________________________________________________________
    


```python
trump_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

hilary_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

obama_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

seinfeld_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

gadot_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])
```


```python
trump_model.fit(trump_input, y=trump_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

hilary_model.fit(hilary_input, y=hilary_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

obama_model.fit(obama_input, y=obama_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

seinfeld_model.fit(seinfeld_input, y=seinfeld_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

gadot_model.fit(gadot_input, y=gadot_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)
```

    Train on 5896 samples, validate on 1475 samples
    Epoch 1/50
    5896/5896 [==============================] - 2s - loss: 2.9996 - acc: 0.0393 - val_loss: 2.7307 - val_acc: 0.0583
    Epoch 2/50
    5896/5896 [==============================] - 2s - loss: 2.8916 - acc: 0.0602 - val_loss: 2.7148 - val_acc: 0.0583
    Epoch 3/50
    5896/5896 [==============================] - 2s - loss: 2.8724 - acc: 0.0488 - val_loss: 2.7075 - val_acc: 0.0583
    Epoch 4/50
    5896/5896 [==============================] - 2s - loss: 2.8443 - acc: 0.0488 - val_loss: 2.6911 - val_acc: 0.0583
    Epoch 5/50
    5896/5896 [==============================] - 2s - loss: 2.8101 - acc: 0.0488 - val_loss: 2.6558 - val_acc: 0.0583
    Epoch 6/50
    5896/5896 [==============================] - 2s - loss: 2.7494 - acc: 0.0987 - val_loss: 2.5932 - val_acc: 0.1288
    Epoch 7/50
    5896/5896 [==============================] - 2s - loss: 2.6668 - acc: 0.1184 - val_loss: 2.5492 - val_acc: 0.1308
    Epoch 8/50
    5896/5896 [==============================] - 2s - loss: 2.5772 - acc: 0.1240 - val_loss: 2.4827 - val_acc: 0.1275
    Epoch 9/50
    5896/5896 [==============================] - 2s - loss: 2.5042 - acc: 0.1355 - val_loss: 2.4662 - val_acc: 0.1010
    Epoch 10/50
    5896/5896 [==============================] - 2s - loss: 2.4330 - acc: 0.1481 - val_loss: 2.4118 - val_acc: 0.1390
    Epoch 11/50
    5896/5896 [==============================] - 2s - loss: 2.3605 - acc: 0.1554 - val_loss: 2.3719 - val_acc: 0.1315
    Epoch 12/50
    5896/5896 [==============================] - 1s - loss: 2.2956 - acc: 0.1625 - val_loss: 2.3570 - val_acc: 0.1200
    Epoch 13/50
    5896/5896 [==============================] - 2s - loss: 2.2356 - acc: 0.1657 - val_loss: 2.3444 - val_acc: 0.1336
    Epoch 14/50
    5896/5896 [==============================] - 2s - loss: 2.1871 - acc: 0.1713 - val_loss: 2.2932 - val_acc: 0.1458
    Epoch 15/50
    5896/5896 [==============================] - 2s - loss: 2.1352 - acc: 0.1759 - val_loss: 2.2904 - val_acc: 0.1336
    Epoch 16/50
    5896/5896 [==============================] - 2s - loss: 2.0946 - acc: 0.1806 - val_loss: 2.2582 - val_acc: 0.1417
    Epoch 17/50
    5896/5896 [==============================] - 2s - loss: 2.0601 - acc: 0.1847 - val_loss: 2.2575 - val_acc: 0.1512
    Epoch 18/50
    5896/5896 [==============================] - 2s - loss: 2.0235 - acc: 0.1893 - val_loss: 2.2480 - val_acc: 0.1532
    Epoch 19/50
    5896/5896 [==============================] - 2s - loss: 1.9951 - acc: 0.1898 - val_loss: 2.2569 - val_acc: 0.1539
    Epoch 20/50
    5896/5896 [==============================] - 2s - loss: 1.9682 - acc: 0.1898 - val_loss: 2.2436 - val_acc: 0.1553
    Epoch 21/50
    5896/5896 [==============================] - 2s - loss: 1.9434 - acc: 0.1923 - val_loss: 2.2423 - val_acc: 0.1505
    Epoch 22/50
    5896/5896 [==============================] - 2s - loss: 1.9237 - acc: 0.1905 - val_loss: 2.2504 - val_acc: 0.1498
    Epoch 23/50
    5896/5896 [==============================] - 2s - loss: 1.9032 - acc: 0.1935 - val_loss: 2.2704 - val_acc: 0.1485
    Epoch 24/50
    5896/5896 [==============================] - 2s - loss: 1.8869 - acc: 0.1911 - val_loss: 2.2695 - val_acc: 0.1356
    Epoch 25/50
    5896/5896 [==============================] - 2s - loss: 1.8696 - acc: 0.1932 - val_loss: 2.2806 - val_acc: 0.1376
    Epoch 26/50
    5896/5896 [==============================] - 2s - loss: 1.8598 - acc: 0.1935 - val_loss: 2.2764 - val_acc: 0.1478
    Epoch 27/50
    5896/5896 [==============================] - 2s - loss: 1.8450 - acc: 0.1922 - val_loss: 2.2890 - val_acc: 0.1525
    Epoch 28/50
    5896/5896 [==============================] - 2s - loss: 1.8356 - acc: 0.1923 - val_loss: 2.2836 - val_acc: 0.1539
    Epoch 29/50
    5896/5896 [==============================] - 2s - loss: 1.8240 - acc: 0.1947 - val_loss: 2.3178 - val_acc: 0.1586
    Epoch 30/50
    5896/5896 [==============================] - 2s - loss: 1.8185 - acc: 0.1939 - val_loss: 2.3145 - val_acc: 0.1553
    Epoch 31/50
    5896/5896 [==============================] - 2s - loss: 1.8101 - acc: 0.1956 - val_loss: 2.3122 - val_acc: 0.1519
    Epoch 32/50
    5896/5896 [==============================] - 2s - loss: 1.8033 - acc: 0.1939 - val_loss: 2.3284 - val_acc: 0.1532
    Epoch 33/50
    5896/5896 [==============================] - 2s - loss: 1.7980 - acc: 0.1932 - val_loss: 2.3501 - val_acc: 0.1532
    Epoch 34/50
    5896/5896 [==============================] - 2s - loss: 1.7924 - acc: 0.1967 - val_loss: 2.3780 - val_acc: 0.1512
    Epoch 35/50
    5896/5896 [==============================] - 2s - loss: 1.7872 - acc: 0.1947 - val_loss: 2.3731 - val_acc: 0.1553
    Epoch 36/50
    5896/5896 [==============================] - 2s - loss: 1.7832 - acc: 0.1939 - val_loss: 2.3703 - val_acc: 0.1566
    Epoch 37/50
    5896/5896 [==============================] - 2s - loss: 1.7804 - acc: 0.1942 - val_loss: 2.3797 - val_acc: 0.1539
    Epoch 38/50
    5896/5896 [==============================] - 2s - loss: 1.7773 - acc: 0.1961 - val_loss: 2.3864 - val_acc: 0.1580
    Epoch 39/50
    5896/5896 [==============================] - 2s - loss: 1.7751 - acc: 0.1959 - val_loss: 2.4085 - val_acc: 0.1539
    Epoch 40/50
    5896/5896 [==============================] - 2s - loss: 1.7718 - acc: 0.1971 - val_loss: 2.4020 - val_acc: 0.1505
    Epoch 41/50
    5896/5896 [==============================] - 2s - loss: 1.7682 - acc: 0.1950 - val_loss: 2.4063 - val_acc: 0.1566
    Epoch 42/50
    5896/5896 [==============================] - 2s - loss: 1.7672 - acc: 0.1939 - val_loss: 2.3865 - val_acc: 0.1525
    Epoch 43/50
    5896/5896 [==============================] - 2s - loss: 1.7654 - acc: 0.1952 - val_loss: 2.4621 - val_acc: 0.1546
    Epoch 44/50
    5896/5896 [==============================] - 2s - loss: 1.7622 - acc: 0.1956 - val_loss: 2.4257 - val_acc: 0.1532
    Epoch 45/50
    5896/5896 [==============================] - 2s - loss: 1.7611 - acc: 0.1949 - val_loss: 2.4491 - val_acc: 0.1566
    Epoch 46/50
    5896/5896 [==============================] - 2s - loss: 1.7608 - acc: 0.1940 - val_loss: 2.4594 - val_acc: 0.1586
    Epoch 47/50
    5896/5896 [==============================] - 2s - loss: 1.7597 - acc: 0.1939 - val_loss: 2.4494 - val_acc: 0.1586
    Epoch 48/50
    5896/5896 [==============================] - 2s - loss: 1.7567 - acc: 0.1947 - val_loss: 2.4586 - val_acc: 0.1573
    Epoch 49/50
    5896/5896 [==============================] - 2s - loss: 1.7582 - acc: 0.1930 - val_loss: 2.4577 - val_acc: 0.1553
    Epoch 50/50
    5896/5896 [==============================] - 2s - loss: 1.7539 - acc: 0.1957 - val_loss: 2.4921 - val_acc: 0.1566
    Train on 3867 samples, validate on 967 samples
    Epoch 1/50
    3867/3867 [==============================] - 1s - loss: 3.0658 - acc: 0.0378 - val_loss: 2.8855 - val_acc: 0.0641
    Epoch 2/50
    3867/3867 [==============================] - 1s - loss: 2.9222 - acc: 0.0509 - val_loss: 2.8416 - val_acc: 0.0641
    Epoch 3/50
    3867/3867 [==============================] - 1s - loss: 2.8990 - acc: 0.0737 - val_loss: 2.8206 - val_acc: 0.1282
    Epoch 4/50
    3867/3867 [==============================] - 1s - loss: 2.8841 - acc: 0.0654 - val_loss: 2.8133 - val_acc: 0.0641
    Epoch 5/50
    3867/3867 [==============================] - 1s - loss: 2.8718 - acc: 0.0509 - val_loss: 2.7930 - val_acc: 0.0641
    Epoch 6/50
    3867/3867 [==============================] - 1s - loss: 2.8452 - acc: 0.0509 - val_loss: 2.7776 - val_acc: 0.0641
    Epoch 7/50
    3867/3867 [==============================] - 1s - loss: 2.8124 - acc: 0.0670 - val_loss: 2.7476 - val_acc: 0.1541
    Epoch 8/50
    3867/3867 [==============================] - 1s - loss: 2.7607 - acc: 0.1275 - val_loss: 2.6918 - val_acc: 0.1582
    Epoch 9/50
    3867/3867 [==============================] - 1s - loss: 2.7075 - acc: 0.1306 - val_loss: 2.6275 - val_acc: 0.1572
    Epoch 10/50
    3867/3867 [==============================] - 1s - loss: 2.6478 - acc: 0.1319 - val_loss: 2.6115 - val_acc: 0.1768
    Epoch 11/50
    3867/3867 [==============================] - 1s - loss: 2.5969 - acc: 0.1376 - val_loss: 2.5728 - val_acc: 0.1375
    Epoch 12/50
    3867/3867 [==============================] - 1s - loss: 2.5416 - acc: 0.1490 - val_loss: 2.5357 - val_acc: 0.1551
    Epoch 13/50
    3867/3867 [==============================] - 1s - loss: 2.4937 - acc: 0.1469 - val_loss: 2.5021 - val_acc: 0.1830
    Epoch 14/50
    3867/3867 [==============================] - 1s - loss: 2.4517 - acc: 0.1539 - val_loss: 2.4437 - val_acc: 0.1768
    Epoch 15/50
    3867/3867 [==============================] - 1s - loss: 2.4008 - acc: 0.1521 - val_loss: 2.4579 - val_acc: 0.1510
    Epoch 16/50
    3867/3867 [==============================] - 1s - loss: 2.3632 - acc: 0.1634 - val_loss: 2.4090 - val_acc: 0.1820
    Epoch 17/50
    3867/3867 [==============================] - 1s - loss: 2.3179 - acc: 0.1663 - val_loss: 2.4057 - val_acc: 0.1892
    Epoch 18/50
    3867/3867 [==============================] - 1s - loss: 2.2822 - acc: 0.1707 - val_loss: 2.3726 - val_acc: 0.1882
    Epoch 19/50
    3867/3867 [==============================] - 1s - loss: 2.2453 - acc: 0.1792 - val_loss: 2.3628 - val_acc: 0.1882
    Epoch 20/50
    3867/3867 [==============================] - 1s - loss: 2.2058 - acc: 0.1831 - val_loss: 2.3461 - val_acc: 0.1903
    Epoch 21/50
    3867/3867 [==============================] - 1s - loss: 2.1739 - acc: 0.1885 - val_loss: 2.3372 - val_acc: 0.1892
    Epoch 22/50
    3867/3867 [==============================] - 1s - loss: 2.1431 - acc: 0.1919 - val_loss: 2.3366 - val_acc: 0.1923
    Epoch 23/50
    3867/3867 [==============================] - 1s - loss: 2.1141 - acc: 0.1896 - val_loss: 2.3245 - val_acc: 0.1944
    Epoch 24/50
    3867/3867 [==============================] - 1s - loss: 2.0835 - acc: 0.1983 - val_loss: 2.3228 - val_acc: 0.1923
    Epoch 25/50
    3867/3867 [==============================] - 1s - loss: 2.0604 - acc: 0.1950 - val_loss: 2.3103 - val_acc: 0.1903
    Epoch 26/50
    3867/3867 [==============================] - 1s - loss: 2.0346 - acc: 0.2004 - val_loss: 2.3348 - val_acc: 0.1655
    Epoch 27/50
    3867/3867 [==============================] - 1s - loss: 2.0076 - acc: 0.1999 - val_loss: 2.3154 - val_acc: 0.1996
    Epoch 28/50
    3867/3867 [==============================] - 1s - loss: 1.9897 - acc: 0.2009 - val_loss: 2.3103 - val_acc: 0.1903
    Epoch 29/50
    3867/3867 [==============================] - 1s - loss: 1.9695 - acc: 0.2002 - val_loss: 2.3098 - val_acc: 0.1872
    Epoch 30/50
    3867/3867 [==============================] - 1s - loss: 1.9497 - acc: 0.2014 - val_loss: 2.3175 - val_acc: 0.1706
    Epoch 31/50
    3867/3867 [==============================] - 1s - loss: 1.9329 - acc: 0.2064 - val_loss: 2.3241 - val_acc: 0.1913
    Epoch 32/50
    3867/3867 [==============================] - 1s - loss: 1.9180 - acc: 0.2038 - val_loss: 2.3189 - val_acc: 0.1913
    Epoch 33/50
    3867/3867 [==============================] - 1s - loss: 1.9022 - acc: 0.2017 - val_loss: 2.3265 - val_acc: 0.1954
    Epoch 34/50
    3867/3867 [==============================] - 1s - loss: 1.8901 - acc: 0.2043 - val_loss: 2.3344 - val_acc: 0.1934
    Epoch 35/50
    3867/3867 [==============================] - 1s - loss: 1.8805 - acc: 0.2038 - val_loss: 2.3476 - val_acc: 0.1903
    Epoch 36/50
    3867/3867 [==============================] - 1s - loss: 1.8661 - acc: 0.2053 - val_loss: 2.3615 - val_acc: 0.1923
    Epoch 37/50
    3867/3867 [==============================] - 1s - loss: 1.8576 - acc: 0.2038 - val_loss: 2.3603 - val_acc: 0.1861
    Epoch 38/50
    3867/3867 [==============================] - 1s - loss: 1.8500 - acc: 0.2035 - val_loss: 2.3816 - val_acc: 0.1872
    Epoch 39/50
    3867/3867 [==============================] - 1s - loss: 1.8394 - acc: 0.2022 - val_loss: 2.3890 - val_acc: 0.1892
    Epoch 40/50
    3867/3867 [==============================] - 1s - loss: 1.8356 - acc: 0.2048 - val_loss: 2.3950 - val_acc: 0.1882
    Epoch 41/50
    3867/3867 [==============================] - 1s - loss: 1.8248 - acc: 0.2017 - val_loss: 2.3777 - val_acc: 0.1892
    Epoch 42/50
    3867/3867 [==============================] - 1s - loss: 1.8189 - acc: 0.2035 - val_loss: 2.3922 - val_acc: 0.1913
    Epoch 43/50
    3867/3867 [==============================] - 1s - loss: 1.8134 - acc: 0.2035 - val_loss: 2.3913 - val_acc: 0.1944
    Epoch 44/50
    3867/3867 [==============================] - 1s - loss: 1.8077 - acc: 0.2043 - val_loss: 2.4078 - val_acc: 0.1882
    Epoch 45/50
    3867/3867 [==============================] - 1s - loss: 1.8038 - acc: 0.2040 - val_loss: 2.4075 - val_acc: 0.1882
    Epoch 46/50
    3867/3867 [==============================] - 1s - loss: 1.8002 - acc: 0.2009 - val_loss: 2.4349 - val_acc: 0.1892
    Epoch 47/50
    3867/3867 [==============================] - 1s - loss: 1.7955 - acc: 0.2051 - val_loss: 2.4434 - val_acc: 0.1851
    Epoch 48/50
    3867/3867 [==============================] - 1s - loss: 1.7893 - acc: 0.2038 - val_loss: 2.4576 - val_acc: 0.1675
    Epoch 49/50
    3867/3867 [==============================] - 1s - loss: 1.7894 - acc: 0.2046 - val_loss: 2.4515 - val_acc: 0.1872
    Epoch 50/50
    3867/3867 [==============================] - 1s - loss: 1.7846 - acc: 0.2038 - val_loss: 2.4551 - val_acc: 0.1913
    Train on 3361 samples, validate on 841 samples
    Epoch 1/50
    3361/3361 [==============================] - 1s - loss: 3.0203 - acc: 0.0479 - val_loss: 3.0420 - val_acc: 0.0559
    Epoch 2/50
    3361/3361 [==============================] - 1s - loss: 2.8505 - acc: 0.0598 - val_loss: 3.0117 - val_acc: 0.0559
    Epoch 3/50
    3361/3361 [==============================] - 1s - loss: 2.8271 - acc: 0.0768 - val_loss: 3.0066 - val_acc: 0.1106
    Epoch 4/50
    3361/3361 [==============================] - 1s - loss: 2.8167 - acc: 0.0934 - val_loss: 2.9873 - val_acc: 0.0559
    Epoch 5/50
    3361/3361 [==============================] - 1s - loss: 2.8029 - acc: 0.0598 - val_loss: 2.9855 - val_acc: 0.0559
    Epoch 6/50
    3361/3361 [==============================] - 1s - loss: 2.7874 - acc: 0.0598 - val_loss: 2.9777 - val_acc: 0.0559
    Epoch 7/50
    3361/3361 [==============================] - 1s - loss: 2.7647 - acc: 0.0598 - val_loss: 2.9470 - val_acc: 0.0559
    Epoch 8/50
    3361/3361 [==============================] - 1s - loss: 2.7326 - acc: 0.0783 - val_loss: 2.9204 - val_acc: 0.1403
    Epoch 9/50
    3361/3361 [==============================] - 1s - loss: 2.6871 - acc: 0.1482 - val_loss: 2.8998 - val_acc: 0.1403
    Epoch 10/50
    3361/3361 [==============================] - 1s - loss: 2.6352 - acc: 0.1544 - val_loss: 2.8287 - val_acc: 0.1403
    Epoch 11/50
    3361/3361 [==============================] - 1s - loss: 2.5777 - acc: 0.1577 - val_loss: 2.8063 - val_acc: 0.1427
    Epoch 12/50
    3361/3361 [==============================] - 1s - loss: 2.5247 - acc: 0.1607 - val_loss: 2.7423 - val_acc: 0.1605
    Epoch 13/50
    3361/3361 [==============================] - 1s - loss: 2.4714 - acc: 0.1657 - val_loss: 2.7057 - val_acc: 0.1546
    Epoch 14/50
    3361/3361 [==============================] - 1s - loss: 2.4266 - acc: 0.1681 - val_loss: 2.6615 - val_acc: 0.1605
    Epoch 15/50
    3361/3361 [==============================] - 1s - loss: 2.3707 - acc: 0.1696 - val_loss: 2.6244 - val_acc: 0.1677
    Epoch 16/50
    3361/3361 [==============================] - 1s - loss: 2.3286 - acc: 0.1747 - val_loss: 2.6033 - val_acc: 0.1546
    Epoch 17/50
    3361/3361 [==============================] - 1s - loss: 2.2805 - acc: 0.1794 - val_loss: 2.5733 - val_acc: 0.1605
    Epoch 18/50
    3361/3361 [==============================] - 1s - loss: 2.2418 - acc: 0.1809 - val_loss: 2.5578 - val_acc: 0.1784
    Epoch 19/50
    3361/3361 [==============================] - 1s - loss: 2.1924 - acc: 0.1907 - val_loss: 2.5482 - val_acc: 0.1819
    Epoch 20/50
    3361/3361 [==============================] - 1s - loss: 2.1537 - acc: 0.2023 - val_loss: 2.5028 - val_acc: 0.1700
    Epoch 21/50
    3361/3361 [==============================] - 1s - loss: 2.1168 - acc: 0.2041 - val_loss: 2.4887 - val_acc: 0.1962
    Epoch 22/50
    3361/3361 [==============================] - 1s - loss: 2.0784 - acc: 0.2077 - val_loss: 2.4802 - val_acc: 0.1962
    Epoch 23/50
    3361/3361 [==============================] - 1s - loss: 2.0374 - acc: 0.2163 - val_loss: 2.4427 - val_acc: 0.2105
    Epoch 24/50
    3361/3361 [==============================] - 1s - loss: 2.0019 - acc: 0.2160 - val_loss: 2.4547 - val_acc: 0.1986
    Epoch 25/50
    3361/3361 [==============================] - 1s - loss: 1.9718 - acc: 0.2166 - val_loss: 2.4061 - val_acc: 0.2105
    Epoch 26/50
    3361/3361 [==============================] - 1s - loss: 1.9412 - acc: 0.2205 - val_loss: 2.4288 - val_acc: 0.2021
    Epoch 27/50
    3361/3361 [==============================] - 1s - loss: 1.9145 - acc: 0.2258 - val_loss: 2.4261 - val_acc: 0.2010
    Epoch 28/50
    3361/3361 [==============================] - 1s - loss: 1.8875 - acc: 0.2276 - val_loss: 2.3919 - val_acc: 0.2069
    Epoch 29/50
    3361/3361 [==============================] - 1s - loss: 1.8658 - acc: 0.2303 - val_loss: 2.4017 - val_acc: 0.2128
    Epoch 30/50
    3361/3361 [==============================] - 1s - loss: 1.8455 - acc: 0.2327 - val_loss: 2.4181 - val_acc: 0.2069
    Epoch 31/50
    3361/3361 [==============================] - 1s - loss: 1.8263 - acc: 0.2339 - val_loss: 2.4065 - val_acc: 0.2093
    Epoch 32/50
    3361/3361 [==============================] - 1s - loss: 1.8130 - acc: 0.2333 - val_loss: 2.3992 - val_acc: 0.2105
    Epoch 33/50
    3361/3361 [==============================] - 1s - loss: 1.7915 - acc: 0.2353 - val_loss: 2.4054 - val_acc: 0.2164
    Epoch 34/50
    3361/3361 [==============================] - 1s - loss: 1.7808 - acc: 0.2362 - val_loss: 2.4385 - val_acc: 0.2117
    Epoch 35/50
    3361/3361 [==============================] - 1s - loss: 1.7651 - acc: 0.2392 - val_loss: 2.4232 - val_acc: 0.2128
    Epoch 36/50
    3361/3361 [==============================] - 1s - loss: 1.7527 - acc: 0.2395 - val_loss: 2.4233 - val_acc: 0.2117
    Epoch 37/50
    3361/3361 [==============================] - 1s - loss: 1.7450 - acc: 0.2395 - val_loss: 2.4312 - val_acc: 0.2128
    Epoch 38/50
    3361/3361 [==============================] - 1s - loss: 1.7332 - acc: 0.2374 - val_loss: 2.4530 - val_acc: 0.2093
    Epoch 39/50
    3361/3361 [==============================] - 1s - loss: 1.7235 - acc: 0.2380 - val_loss: 2.4661 - val_acc: 0.2188
    Epoch 40/50
    3361/3361 [==============================] - 1s - loss: 1.7174 - acc: 0.2362 - val_loss: 2.4589 - val_acc: 0.2152
    Epoch 41/50
    3361/3361 [==============================] - 1s - loss: 1.7073 - acc: 0.2371 - val_loss: 2.4621 - val_acc: 0.2152
    Epoch 42/50
    3361/3361 [==============================] - 1s - loss: 1.7000 - acc: 0.2395 - val_loss: 2.4915 - val_acc: 0.2128
    Epoch 43/50
    3361/3361 [==============================] - 1s - loss: 1.6941 - acc: 0.2345 - val_loss: 2.4811 - val_acc: 0.2152
    Epoch 44/50
    3361/3361 [==============================] - 1s - loss: 1.6869 - acc: 0.2359 - val_loss: 2.4954 - val_acc: 0.2164
    Epoch 45/50
    3361/3361 [==============================] - 1s - loss: 1.6830 - acc: 0.2398 - val_loss: 2.5109 - val_acc: 0.2164
    Epoch 46/50
    3361/3361 [==============================] - 1s - loss: 1.6777 - acc: 0.2377 - val_loss: 2.5100 - val_acc: 0.2152
    Epoch 47/50
    3361/3361 [==============================] - 1s - loss: 1.6728 - acc: 0.2359 - val_loss: 2.5207 - val_acc: 0.2128
    Epoch 48/50
    3361/3361 [==============================] - 1s - loss: 1.6709 - acc: 0.2395 - val_loss: 2.5511 - val_acc: 0.21170.2
    Epoch 49/50
    3361/3361 [==============================] - 1s - loss: 1.6642 - acc: 0.2371 - val_loss: 2.5357 - val_acc: 0.2140
    Epoch 50/50
    3361/3361 [==============================] - 1s - loss: 1.6618 - acc: 0.2389 - val_loss: 2.5531 - val_acc: 0.2140
    Train on 3031 samples, validate on 758 samples
    Epoch 1/50
    3031/3031 [==============================] - 1s - loss: 3.2261 - acc: 0.0587 - val_loss: 2.8259 - val_acc: 0.0831
    Epoch 2/50
    3031/3031 [==============================] - 1s - loss: 2.9744 - acc: 0.0940 - val_loss: 2.7662 - val_acc: 0.0831
    Epoch 3/50
    3031/3031 [==============================] - 1s - loss: 2.9276 - acc: 0.0986 - val_loss: 2.7675 - val_acc: 0.0831
    Epoch 4/50
    3031/3031 [==============================] - 1s - loss: 2.9068 - acc: 0.0940 - val_loss: 2.7376 - val_acc: 0.0831
    Epoch 5/50
    3031/3031 [==============================] - 1s - loss: 2.8819 - acc: 0.0940 - val_loss: 2.7470 - val_acc: 0.0831
    Epoch 6/50
    3031/3031 [==============================] - 1s - loss: 2.8627 - acc: 0.0940 - val_loss: 2.7101 - val_acc: 0.0831
    Epoch 7/50
    3031/3031 [==============================] - 1s - loss: 2.8166 - acc: 0.1603 - val_loss: 2.6820 - val_acc: 0.0831
    Epoch 8/50
    3031/3031 [==============================] - 1s - loss: 2.7616 - acc: 0.2003 - val_loss: 2.6292 - val_acc: 0.2111
    Epoch 9/50
    3031/3031 [==============================] - 1s - loss: 2.6977 - acc: 0.2174 - val_loss: 2.5524 - val_acc: 0.1979
    Epoch 10/50
    3031/3031 [==============================] - 1s - loss: 2.6314 - acc: 0.2243 - val_loss: 2.5119 - val_acc: 0.2335
    Epoch 11/50
    3031/3031 [==============================] - 1s - loss: 2.5673 - acc: 0.2329 - val_loss: 2.4416 - val_acc: 0.2454
    Epoch 12/50
    3031/3031 [==============================] - 1s - loss: 2.4996 - acc: 0.2474 - val_loss: 2.4454 - val_acc: 0.2335
    Epoch 13/50
    3031/3031 [==============================] - 1s - loss: 2.4449 - acc: 0.2567 - val_loss: 2.3550 - val_acc: 0.2335
    Epoch 14/50
    3031/3031 [==============================] - 1s - loss: 2.3917 - acc: 0.2577 - val_loss: 2.3417 - val_acc: 0.2559
    Epoch 15/50
    3031/3031 [==============================] - 1s - loss: 2.3421 - acc: 0.2597 - val_loss: 2.3209 - val_acc: 0.2546
    Epoch 16/50
    3031/3031 [==============================] - 1s - loss: 2.2886 - acc: 0.2600 - val_loss: 2.2742 - val_acc: 0.2533
    Epoch 17/50
    3031/3031 [==============================] - 1s - loss: 2.2528 - acc: 0.2610 - val_loss: 2.2971 - val_acc: 0.2546
    Epoch 18/50
    3031/3031 [==============================] - 1s - loss: 2.2008 - acc: 0.2626 - val_loss: 2.2866 - val_acc: 0.2533
    Epoch 19/50
    3031/3031 [==============================] - 1s - loss: 2.1630 - acc: 0.2639 - val_loss: 2.2303 - val_acc: 0.2533
    Epoch 20/50
    3031/3031 [==============================] - 1s - loss: 2.1226 - acc: 0.2646 - val_loss: 2.2328 - val_acc: 0.2573
    Epoch 21/50
    3031/3031 [==============================] - 1s - loss: 2.0861 - acc: 0.2712 - val_loss: 2.2224 - val_acc: 0.2652
    Epoch 22/50
    3031/3031 [==============================] - 1s - loss: 2.0544 - acc: 0.2758 - val_loss: 2.1750 - val_acc: 0.2691
    Epoch 23/50
    3031/3031 [==============================] - 1s - loss: 2.0173 - acc: 0.2808 - val_loss: 2.1809 - val_acc: 0.2559
    Epoch 24/50
    3031/3031 [==============================] - 1s - loss: 1.9837 - acc: 0.2785 - val_loss: 2.1901 - val_acc: 0.2639
    Epoch 25/50
    3031/3031 [==============================] - 1s - loss: 1.9561 - acc: 0.2841 - val_loss: 2.1354 - val_acc: 0.2652
    Epoch 26/50
    3031/3031 [==============================] - 1s - loss: 1.9274 - acc: 0.2897 - val_loss: 2.1452 - val_acc: 0.2639
    Epoch 27/50
    3031/3031 [==============================] - 1s - loss: 1.9044 - acc: 0.2900 - val_loss: 2.1496 - val_acc: 0.2665
    Epoch 28/50
    3031/3031 [==============================] - 1s - loss: 1.8802 - acc: 0.2940 - val_loss: 2.1268 - val_acc: 0.2652
    Epoch 29/50
    3031/3031 [==============================] - 1s - loss: 1.8544 - acc: 0.2953 - val_loss: 2.1412 - val_acc: 0.2612
    Epoch 30/50
    3031/3031 [==============================] - 1s - loss: 1.8344 - acc: 0.2992 - val_loss: 2.1593 - val_acc: 0.2625
    Epoch 31/50
    3031/3031 [==============================] - 1s - loss: 1.8123 - acc: 0.3009 - val_loss: 2.1354 - val_acc: 0.2599
    Epoch 32/50
    3031/3031 [==============================] - 1s - loss: 1.7932 - acc: 0.3019 - val_loss: 2.1476 - val_acc: 0.2573
    Epoch 33/50
    3031/3031 [==============================] - 1s - loss: 1.7757 - acc: 0.3042 - val_loss: 2.1459 - val_acc: 0.2665
    Epoch 34/50
    3031/3031 [==============================] - 1s - loss: 1.7600 - acc: 0.3055 - val_loss: 2.1468 - val_acc: 0.2559
    Epoch 35/50
    3031/3031 [==============================] - 1s - loss: 1.7444 - acc: 0.3032 - val_loss: 2.1537 - val_acc: 0.2639
    Epoch 36/50
    3031/3031 [==============================] - 1s - loss: 1.7291 - acc: 0.3078 - val_loss: 2.1871 - val_acc: 0.2573
    Epoch 37/50
    3031/3031 [==============================] - 1s - loss: 1.7194 - acc: 0.3058 - val_loss: 2.1872 - val_acc: 0.2652
    Epoch 38/50
    3031/3031 [==============================] - 1s - loss: 1.7035 - acc: 0.3098 - val_loss: 2.1766 - val_acc: 0.2559
    Epoch 39/50
    3031/3031 [==============================] - 1s - loss: 1.6913 - acc: 0.3078 - val_loss: 2.1924 - val_acc: 0.2454
    Epoch 40/50
    3031/3031 [==============================] - 1s - loss: 1.6782 - acc: 0.3098 - val_loss: 2.1976 - val_acc: 0.2441
    Epoch 41/50
    3031/3031 [==============================] - 1s - loss: 1.6696 - acc: 0.3072 - val_loss: 2.1948 - val_acc: 0.2533
    Epoch 42/50
    3031/3031 [==============================] - 1s - loss: 1.6569 - acc: 0.3114 - val_loss: 2.2084 - val_acc: 0.2546
    Epoch 43/50
    3031/3031 [==============================] - 1s - loss: 1.6490 - acc: 0.3091 - val_loss: 2.2171 - val_acc: 0.2612
    Epoch 44/50
    3031/3031 [==============================] - 1s - loss: 1.6425 - acc: 0.3128 - val_loss: 2.2198 - val_acc: 0.2573
    Epoch 45/50
    3031/3031 [==============================] - 1s - loss: 1.6330 - acc: 0.3111 - val_loss: 2.2302 - val_acc: 0.2559
    Epoch 46/50
    3031/3031 [==============================] - 1s - loss: 1.6249 - acc: 0.3108 - val_loss: 2.2349 - val_acc: 0.2559
    Epoch 47/50
    3031/3031 [==============================] - 1s - loss: 1.6165 - acc: 0.3131 - val_loss: 2.2528 - val_acc: 0.2559
    Epoch 48/50
    3031/3031 [==============================] - 1s - loss: 1.6115 - acc: 0.3124 - val_loss: 2.2575 - val_acc: 0.2507
    Epoch 49/50
    3031/3031 [==============================] - 1s - loss: 1.6059 - acc: 0.3161 - val_loss: 2.2724 - val_acc: 0.2388
    Epoch 50/50
    3031/3031 [==============================] - 1s - loss: 1.6037 - acc: 0.3124 - val_loss: 2.2695 - val_acc: 0.2520
    Train on 1921 samples, validate on 481 samples
    Epoch 1/50
    1921/1921 [==============================] - 0s - loss: 3.5780 - acc: 0.0193 - val_loss: 3.2057 - val_acc: 0.0416
    Epoch 2/50
    1921/1921 [==============================] - 0s - loss: 3.3584 - acc: 0.0427 - val_loss: 3.1440 - val_acc: 0.1185
    Epoch 3/50
    1921/1921 [==============================] - 0s - loss: 3.3177 - acc: 0.0880 - val_loss: 3.1497 - val_acc: 0.0769
    Epoch 4/50
    1921/1921 [==============================] - 0s - loss: 3.2989 - acc: 0.0843 - val_loss: 3.1232 - val_acc: 0.0769
    Epoch 5/50
    1921/1921 [==============================] - 0s - loss: 3.2823 - acc: 0.0843 - val_loss: 3.1164 - val_acc: 0.0769
    Epoch 6/50
    1921/1921 [==============================] - 0s - loss: 3.2735 - acc: 0.0843 - val_loss: 3.1182 - val_acc: 0.0769
    Epoch 7/50
    1921/1921 [==============================] - 0s - loss: 3.2534 - acc: 0.0843 - val_loss: 3.1014 - val_acc: 0.0769
    Epoch 8/50
    1921/1921 [==============================] - 0s - loss: 3.2381 - acc: 0.0843 - val_loss: 3.1039 - val_acc: 0.0769
    Epoch 9/50
    1921/1921 [==============================] - 0s - loss: 3.2180 - acc: 0.0843 - val_loss: 3.0758 - val_acc: 0.0769
    Epoch 10/50
    1921/1921 [==============================] - 1s - loss: 3.1928 - acc: 0.0843 - val_loss: 3.0598 - val_acc: 0.0769
    Epoch 11/50
    1921/1921 [==============================] - 1s - loss: 3.1734 - acc: 0.0843 - val_loss: 3.0538 - val_acc: 0.0769
    Epoch 12/50
    1921/1921 [==============================] - 1s - loss: 3.1524 - acc: 0.0843 - val_loss: 3.0161 - val_acc: 0.0769
    Epoch 13/50
    1921/1921 [==============================] - 0s - loss: 3.1211 - acc: 0.0843 - val_loss: 3.0053 - val_acc: 0.0769
    Epoch 14/50
    1921/1921 [==============================] - 1s - loss: 3.0950 - acc: 0.0911 - val_loss: 2.9815 - val_acc: 0.0977
    Epoch 15/50
    1921/1921 [==============================] - 1s - loss: 3.0589 - acc: 0.0984 - val_loss: 2.9559 - val_acc: 0.1414
    Epoch 16/50
    1921/1921 [==============================] - 0s - loss: 3.0236 - acc: 0.1395 - val_loss: 2.9174 - val_acc: 0.1393
    Epoch 17/50
    1921/1921 [==============================] - 0s - loss: 2.9740 - acc: 0.1478 - val_loss: 2.9132 - val_acc: 0.1455
    Epoch 18/50
    1921/1921 [==============================] - 0s - loss: 2.9374 - acc: 0.1520 - val_loss: 2.8883 - val_acc: 0.1455
    Epoch 19/50
    1921/1921 [==============================] - 0s - loss: 2.8957 - acc: 0.1530 - val_loss: 2.8414 - val_acc: 0.1476
    Epoch 20/50
    1921/1921 [==============================] - 0s - loss: 2.8497 - acc: 0.1567 - val_loss: 2.8264 - val_acc: 0.1455
    Epoch 21/50
    1921/1921 [==============================] - 0s - loss: 2.8031 - acc: 0.1567 - val_loss: 2.8446 - val_acc: 0.1476
    Epoch 22/50
    1921/1921 [==============================] - 0s - loss: 2.7605 - acc: 0.1671 - val_loss: 2.8094 - val_acc: 0.1476
    Epoch 23/50
    1921/1921 [==============================] - 0s - loss: 2.7182 - acc: 0.1692 - val_loss: 2.7503 - val_acc: 0.1684
    Epoch 24/50
    1921/1921 [==============================] - 0s - loss: 2.6681 - acc: 0.1806 - val_loss: 2.7571 - val_acc: 0.1663
    Epoch 25/50
    1921/1921 [==============================] - 0s - loss: 2.6354 - acc: 0.1879 - val_loss: 2.7355 - val_acc: 0.1559
    Epoch 26/50
    1921/1921 [==============================] - 0s - loss: 2.5992 - acc: 0.1848 - val_loss: 2.6865 - val_acc: 0.1746
    Epoch 27/50
    1921/1921 [==============================] - 0s - loss: 2.5520 - acc: 0.1916 - val_loss: 2.6910 - val_acc: 0.1726
    Epoch 28/50
    1921/1921 [==============================] - 0s - loss: 2.5238 - acc: 0.1963 - val_loss: 2.6518 - val_acc: 0.1726
    Epoch 29/50
    1921/1921 [==============================] - 0s - loss: 2.4733 - acc: 0.2051 - val_loss: 2.6502 - val_acc: 0.1746
    Epoch 30/50
    1921/1921 [==============================] - 0s - loss: 2.4407 - acc: 0.2041 - val_loss: 2.6398 - val_acc: 0.1726
    Epoch 31/50
    1921/1921 [==============================] - 0s - loss: 2.4044 - acc: 0.2166 - val_loss: 2.6630 - val_acc: 0.1705
    Epoch 32/50
    1921/1921 [==============================] - 0s - loss: 2.3707 - acc: 0.2171 - val_loss: 2.6170 - val_acc: 0.1601
    Epoch 33/50
    1921/1921 [==============================] - 0s - loss: 2.3344 - acc: 0.2186 - val_loss: 2.6247 - val_acc: 0.1705
    Epoch 34/50
    1921/1921 [==============================] - 0s - loss: 2.3071 - acc: 0.2254 - val_loss: 2.6270 - val_acc: 0.1726
    Epoch 35/50
    1921/1921 [==============================] - 0s - loss: 2.2741 - acc: 0.2223 - val_loss: 2.5792 - val_acc: 0.1809
    Epoch 36/50
    1921/1921 [==============================] - 0s - loss: 2.2385 - acc: 0.2353 - val_loss: 2.6018 - val_acc: 0.1622
    Epoch 37/50
    1921/1921 [==============================] - 0s - loss: 2.2130 - acc: 0.2369 - val_loss: 2.5936 - val_acc: 0.1746
    Epoch 38/50
    1921/1921 [==============================] - 0s - loss: 2.1807 - acc: 0.2389 - val_loss: 2.5633 - val_acc: 0.1726
    Epoch 39/50
    1921/1921 [==============================] - 0s - loss: 2.1568 - acc: 0.2436 - val_loss: 2.6047 - val_acc: 0.1767
    Epoch 40/50
    1921/1921 [==============================] - 0s - loss: 2.1328 - acc: 0.2405 - val_loss: 2.6079 - val_acc: 0.1830
    Epoch 41/50
    1921/1921 [==============================] - 0s - loss: 2.1052 - acc: 0.2452 - val_loss: 2.5852 - val_acc: 0.1809
    Epoch 42/50
    1921/1921 [==============================] - 0s - loss: 2.0906 - acc: 0.2410 - val_loss: 2.5982 - val_acc: 0.1788
    Epoch 43/50
    1921/1921 [==============================] - 0s - loss: 2.0620 - acc: 0.2478 - val_loss: 2.5899 - val_acc: 0.1726
    Epoch 44/50
    1921/1921 [==============================] - 0s - loss: 2.0414 - acc: 0.2493 - val_loss: 2.5876 - val_acc: 0.1809
    Epoch 45/50
    1921/1921 [==============================] - 0s - loss: 2.0202 - acc: 0.2462 - val_loss: 2.5888 - val_acc: 0.1726
    Epoch 46/50
    1921/1921 [==============================] - 0s - loss: 2.0024 - acc: 0.2566 - val_loss: 2.6073 - val_acc: 0.1850
    Epoch 47/50
    1921/1921 [==============================] - 0s - loss: 1.9866 - acc: 0.2493 - val_loss: 2.6221 - val_acc: 0.1767
    Epoch 48/50
    1921/1921 [==============================] - 0s - loss: 1.9656 - acc: 0.2488 - val_loss: 2.6090 - val_acc: 0.1871
    Epoch 49/50
    1921/1921 [==============================] - 0s - loss: 1.9514 - acc: 0.2452 - val_loss: 2.6135 - val_acc: 0.1809
    Epoch 50/50
    1921/1921 [==============================] - 0s - loss: 1.9375 - acc: 0.2535 - val_loss: 2.6040 - val_acc: 0.1830
    




    <keras.callbacks.History at 0x1e0060ea5f8>




## Evaluate model on test data



```python
trump_score = trump_model.evaluate(trump_input,trump_output, verbose=0)
print('trump score : ', trump_score)

hilary_score = hilary_model.evaluate(hilary_input,hilary_output, verbose=0)
print('hilary score : ', hilary_score)

obama_score = obama_model.evaluate(obama_input,obama_output, verbose=0)
print('obama score : ', obama_score)

seinfeld_score = seinfeld_model.evaluate(seinfeld_input,seinfeld_output, verbose=0)
print('seinfeld score : ', seinfeld_score)

gadot_score = gadot_model.evaluate(gadot_input,gadot_output, verbose=0)
print('gadot score : ', gadot_score)
```

    trump score :  [1.8794326599312838, 0.19088319089532044]
    hilary score :  [1.8948769167922577, 0.20728175424079437]
    obama score :  [1.8117561166709744, 0.2367920038112568]
    seinfeld score :  [1.70972471804756, 0.3048297704272926]
    gadot score :  [2.0259302889278388, 0.24646128226477934]
    

we will define a function for getting the next word 


```python
def get_next(text,token,model,vocabulary):
    '''Predicts the following word, given a text word, a tokenizer to convert it to 1-hot vector, a trained model and a vocabulary
    with word and index representations'''
    #converting the word to 1-hot matrix represenation
    tmp = text_to_word_sequence(text, lower=False, split=" ")
    tmp = token.texts_to_matrix(tmp, mode='binary')
    #predicting next word
    p = model.predict(tmp)[0]
    match = find_random_word_index(p)
#     print(str(match))
#     print(vocabulary[match])
    return vocabulary[vocabulary['code']==match]['word'].values[0]
```


```python
import random

def find_random_word_index(v):
    found = False
    while found == False:
        index_rand_choice = random.randint(0, len(v) - 1)
        if v[index_rand_choice] != 0:
            return index_rand_choice
    return None
```


```python
def create_sentence(token, model, vocab):
    prev_word = 'NEWTWEET'
    next_word = ''
    res = '';
    count_words = 0
    while next_word.strip() != 'NEWTWEET' and count_words < 30:
        next_word = get_next(prev_word, token, model, vocab)
        res = res + " " + next_word
        prev_word = next_word
        count_words += 1
    return res.rsplit(' ', 1)[0]
```

this is just a check that the function works


```python
print(get_next('Democrats', trump_tokenizer, trump_model, trump_vocab))

print(get_next('Democrats', hilary_tokenizer, hilary_model, hilary_vocab))

print(get_next('Democrats', obama_tokenizer, obama_model, obama_vocab))

print(get_next('Democrats', seinfeld_tokenizer, seinfeld_model, seinfeld_vocab))

print(get_next('Democrats', gadot_tokenizer, gadot_model, gadot_vocab))
```

    are
    your
    Supreme
    all
    wonderful
    


```python
all_trump_tweets = []
all_hilary_tweets = []
all_obama_tweets = []
all_seinfeld_tweets = []
all_gadot_tweets = []

for i in range(0, 150):
    trump_tweet = create_sentence(trump_tokenizer, trump_model, trump_vocab)
    trump_tweet_after_stem = stem_sentence(trump_tweet)
    all_trump_tweets.extend([trump_tweet_after_stem])

for i in range(0, 150):
    hilary_tweet = create_sentence(hilary_tokenizer, hilary_model, hilary_vocab)
    hilary_tweet_after_stem = stem_sentence(hilary_tweet)
    all_hilary_tweets.extend([hilary_tweet_after_stem])
    
for i in range(0, 150):
    obama_tweet = create_sentence(obama_tokenizer, obama_model, obama_vocab)
    obama_tweet_after_stem = stem_sentence(obama_tweet)
    all_obama_tweets.extend([obama_tweet_after_stem])

for i in range(0, 150):
    seinfeld_tweet = create_sentence(seinfeld_tokenizer, seinfeld_model, seinfeld_vocab)
    seinfeld_tweet_after_stem = stem_sentence(seinfeld_tweet)
    all_seinfeld_tweets.extend([seinfeld_tweet_after_stem])

for i in range(0, 150):
    gadot_tweet = create_sentence(gadot_tokenizer, gadot_model, gadot_vocab)
    gadot_tweet_after_stem = stem_sentence(gadot_tweet)
    all_gadot_tweets.extend([gadot_tweet_after_stem])

all_trump_tweets.extend(alltweets_text)
all_hilary_tweets.extend(alltweets_text)
all_obama_tweets.extend(alltweets_text)
all_seinfeld_tweets.extend(alltweets_text)
all_gadot_tweets.extend(alltweets_text)
all_trump_tweets_BOW = create_bag_of_words(all_trump_tweets)
all_hilary_tweets_BOW = create_bag_of_words(all_hilary_tweets)
all_obama_tweets_BOW = create_bag_of_words(all_obama_tweets)
all_seinfeld_tweets_BOW = create_bag_of_words(all_seinfeld_tweets)
all_gadot_tweets_BOW = create_bag_of_words(all_gadot_tweets)

```

## Naiva Bayes Result


```python
trump_right = 0
hilary_right = 0
seinfeld_right = 0
obama_right = 0
gadot_right = 0

for i in range(0, 150):
    if nb.predict([all_trump_tweets_BOW[i]]) == 'Donald J. Trump' :
        trump_right = trump_right + 1
    if nb.predict([all_hilary_tweets_BOW[i]]) == 'Hillary Clinton' :
        hilary_right = hilary_right + 1
    if nb.predict([all_obama_tweets_BOW[i]]) == 'Barack Obama' :
        obama_right = obama_right + 1
    if nb.predict([all_seinfeld_tweets_BOW[i]]) == 'Jerry Seinfeld' :
        seinfeld_right = seinfeld_right + 1
    if nb.predict([all_gadot_tweets_BOW[i]]) == 'Gal Gadot' :
        gadot_right = gadot_right + 1
    
print ("We Succeded in: " + str((trump_right/150)*100) + " precent in prediction which tweets are of Trump from the tweets we generated for him")
print ("We Succeded in: " + str((hilary_right/150)*100) + " precent in prediction which tweets are of Hilary from the tweets we generated for her")
print ("We Succeded in: " + str((obama_right/150)*100) + " precent in prediction which tweets are of obama from the tweets we generated for him")
print ("We Succeded in: " + str((seinfeld_right/150)*100) + " precent in prediction which tweets are of Seinfeld from the tweets we generated for him")
print ("We Succeded in: " + str((gadot_right/150)*100) + " precent in prediction which tweets are of Gal Gadot from the tweets we generated for her")
```

    We Succeded in: 83.33333333333334 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 42.0 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 54.0 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 26.0 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 2.666666666666667 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    

## Logistic Regression Result


```python
trump_right = 0
hilary_right = 0
seinfeld_right = 0
obama_right = 0
gadot_right = 0

for i in range(0, 150):
    if regresion.predict([all_trump_tweets_BOW[i]]) == 'Donald J. Trump' :
        trump_right = trump_right + 1
    if regresion.predict([all_hilary_tweets_BOW[i]]) == 'Hillary Clinton' :
        hilary_right = hilary_right + 1
    if regresion.predict([all_obama_tweets_BOW[i]]) == 'Barack Obama' :
        obama_right = obama_right + 1
    if regresion.predict([all_seinfeld_tweets_BOW[i]]) == 'Jerry Seinfeld' :
        seinfeld_right = seinfeld_right + 1
    if regresion.predict([all_gadot_tweets_BOW[i]]) == 'Gal Gadot' :
        gadot_right = gadot_right + 1
    
print ("We Succeded in: " + str((trump_right/150)*100) + " precent in prediction which tweets are of Trump from the tweets we generated for him")
print ("We Succeded in: " + str((hilary_right/150)*100) + " precent in prediction which tweets are of Hilary from the tweets we generated for her")
print ("We Succeded in: " + str((obama_right/150)*100) + " precent in prediction which tweets are of obama from the tweets we generated for him")
print ("We Succeded in: " + str((seinfeld_right/150)*100) + " precent in prediction which tweets are of Seinfeld from the tweets we generated for him")
print ("We Succeded in: " + str((gadot_right/150)*100) + " precent in prediction which tweets are of Gal Gadot from the tweets we generated for her")
```

    We Succeded in: 68.0 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 50.66666666666667 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 40.0 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 46.666666666666664 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 34.0 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    

## SVC Result


```python
trump_right = 0
hilary_right = 0
seinfeld_right = 0
obama_right = 0
gadot_right = 0

for i in range(0, 150):
    if svc.predict([all_trump_tweets_BOW[i]]) == 'Donald J. Trump' :
        trump_right = trump_right + 1
    if svc.predict([all_hilary_tweets_BOW[i]]) == 'Hillary Clinton' :
        hilary_right = hilary_right + 1
    if svc.predict([all_obama_tweets_BOW[i]]) == 'Barack Obama' :
        obama_right = obama_right + 1
    if svc.predict([all_seinfeld_tweets_BOW[i]]) == 'Jerry Seinfeld' :
        seinfeld_right = seinfeld_right + 1
    if svc.predict([all_gadot_tweets_BOW[i]]) == 'Gal Gadot' :
        gadot_right = gadot_right + 1
    
print ("We Succeded in: " + str((trump_right/150)*100) + " precent in prediction which tweets are of Trump from the tweets we generated for him")
print ("We Succeded in: " + str((hilary_right/150)*100) + " precent in prediction which tweets are of Hilary from the tweets we generated for her")
print ("We Succeded in: " + str((obama_right/150)*100) + " precent in prediction which tweets are of obama from the tweets we generated for him")
print ("We Succeded in: " + str((seinfeld_right/150)*100) + " precent in prediction which tweets are of Seinfeld from the tweets we generated for him")
print ("We Succeded in: " + str((gadot_right/150)*100) + " precent in prediction which tweets are of Gal Gadot from the tweets we generated for her")
```

    We Succeded in: 52.0 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 45.33333333333333 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 45.33333333333333 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 38.0 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 35.333333333333336 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    

## KNN Result


```python
trump_right = 0
hilary_right = 0
seinfeld_right = 0
obama_right = 0
gadot_right = 0

for i in range(0, 150):
    if knn.predict([all_trump_tweets_BOW[i]]) == 'Donald J. Trump' :
        trump_right = trump_right + 1
    if knn.predict([all_hilary_tweets_BOW[i]]) == 'Hillary Clinton' :
        hilary_right = hilary_right + 1
    if knn.predict([all_obama_tweets_BOW[i]]) == 'Barack Obama' :
        obama_right = obama_right + 1
    if knn.predict([all_seinfeld_tweets_BOW[i]]) == 'Jerry Seinfeld' :
        seinfeld_right = seinfeld_right + 1
    if knn.predict([all_gadot_tweets_BOW[i]]) == 'Gal Gadot' :
        gadot_right = gadot_right + 1
    
print ("We Succeded in: " + str((trump_right/150)*100) + " precent in prediction which tweets are of Trump from the tweets we generated for him")
print ("We Succeded in: " + str((hilary_right/150)*100) + " precent in prediction which tweets are of Hilary from the tweets we generated for her")
print ("We Succeded in: " + str((obama_right/150)*100) + " precent in prediction which tweets are of obama from the tweets we generated for him")
print ("We Succeded in: " + str((seinfeld_right/150)*100) + " precent in prediction which tweets are of Seinfeld from the tweets we generated for him")
print ("We Succeded in: " + str((gadot_right/150)*100) + " precent in prediction which tweets are of Gal Gadot from the tweets we generated for her")
```

    We Succeded in: 0.0 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 0.0 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 0.0 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 2.0 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 99.33333333333333 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    

## Random Forest Result


```python
trump_right = 0
hilary_right = 0
seinfeld_right = 0
obama_right = 0
gadot_right = 0

for i in range(0, 150):
    if forest.predict([all_trump_tweets_BOW[i]]) == 'Donald J. Trump' :
        trump_right = trump_right + 1
    if forest.predict([all_hilary_tweets_BOW[i]]) == 'Hillary Clinton' :
        hilary_right = hilary_right + 1
    if forest.predict([all_obama_tweets_BOW[i]]) == 'Barack Obama' :
        obama_right = obama_right + 1
    if forest.predict([all_seinfeld_tweets_BOW[i]]) == 'Jerry Seinfeld' :
        seinfeld_right = seinfeld_right + 1
    if forest.predict([all_gadot_tweets_BOW[i]]) == 'Gal Gadot' :
        gadot_right = gadot_right + 1
    
print ("We Succeded in: " + str((trump_right/150)*100) + " precent in prediction which tweets are of Trump from the tweets we generated for him")
print ("We Succeded in: " + str((hilary_right/150)*100) + " precent in prediction which tweets are of Hilary from the tweets we generated for her")
print ("We Succeded in: " + str((obama_right/150)*100) + " precent in prediction which tweets are of obama from the tweets we generated for him")
print ("We Succeded in: " + str((seinfeld_right/150)*100) + " precent in prediction which tweets are of Seinfeld from the tweets we generated for him")
print ("We Succeded in: " + str((gadot_right/150)*100) + " precent in prediction which tweets are of Gal Gadot from the tweets we generated for her")
```

    We Succeded in: 54.0 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 26.666666666666668 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 25.333333333333336 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 35.333333333333336 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 72.0 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    