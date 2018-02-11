
## Students: Hod Bublil, Achiad Gelerenter.

## IDS: 305212466, 305231995.



now we will download the nltk library in purpose of use this library.


```python
nltk.download()
```

now we will install the tweepy library in purpose of use this library, in order to get data from tweeter API.


```python
!pip install tweepy
```

after download nltk we need to install it.


```python
!pip install nltk
```

now we will install the plotly library in purpose of use this library, in order to print diagrams in the future.


```python
!pip install plotly
```

# PART 1

We will connect to tweeter API using tweepy, we do this by using keys and tokens that we get in the time we opened tweeter application


```python
import tweepy
%run keys.ipynb


auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

```

get 50 most recent tweets of Donald Trump, Hillary Clinton, Barack Obama, Jerry Seinfeld, Gal Gadot.
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
    

Removes all hashtags, @, http words and # in order for the NLP to work better.
We also substitute each non letter char by " " .


```python
import re

def clean_text(text):
    text_updated = " ".join(filter(lambda x: '#' not in x and '@' not in x and 'http' not in x, text.split()))
    return re.sub("[^a-zA-Z']",   # The pattern to search for: in this case- all characters but english letters
                      " ",                   # The pattern to replace it with
                      text_updated)
```

We will create a function for stemming a sentence in order to make the words in their original meaning.
It will improve our accuracy. We do this by using nltk library we downloaded before.


```python
import nltk

porter = nltk.PorterStemmer()
lancaster = nltk.LancasterStemmer()

def stem_sentence(sentence):
    return " ".join([lancaster.stem(w) for w in sentence.split(" ")])

```

From the tweet object we will save only the text and the tweet's writer, after stemming and cleaning the text.


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

    ['my admin has ident three maj pri for cre a saf  modern and law immigr system  ful sec the bord  end chain migr  and cancel the vis lottery  congress must sec the immigr system and protect am ', 'my thought and pray ar with the two pol off  their famy  and everybody at the', 'republ want to fix dac far mor than the democr do  the dem had al three branch of govern back in            and they decid not to do anyth about dac  they on want to us it as a campaign issu  vot republ ', '    ag  not just the fbi  amp  doj  now the stat depart to dig up dirt on him in the day lead up to the elect  comey had convers with donald trump  which i don t believ wer acc   he leak inform  corrupt    tom fitton of jud watch on', ' my view is that not on has trump been vind in the last sev week about the mishandl of the dossy and the lie about the clinton dnc dossy  it show that he s been victim  he s been victim by the obam admin who wer us al sort of       ', 'peopl liv ar being shat and destroy by a mer alleg  som ar tru and som ar fals  som ar old and som ar new  ther is no recovery for someon fals accus   lif and car ar gon  is ther no such thing any long as due process ', 'accord to the a russ sold phony secret on  trump  to the u s  ask pric was     mil  brought down to    mil to be paid ov tim  i hop peopl ar now see  amp  understand what is going on her  it is al now start to com out   drain the swamp ', 'the democr sent a very polit and long respons memo which they knew  becaus of sourc and method  and mor   would hav to be heavy redact  whereupon they would blam the whit hous for lack of transp  told them to re do and send back in prop form ', 'jobless claim hav drop to a    year low ', 'cost on non milit lin wil nev com down if we do not elect mor republ in the      elect  and beyond  thi bil is a big vict for our milit  but much wast in ord to get dem vot  fortun  dac not includ in thi bil  negoty to start now ', 'without mor republ in congress  we wer forc to increas spend on thing we do not lik or want in ord to fin  aft many year of deplet  tak car of our milit  sad  we nee som dem vot for pass  must elect mor republ in      elect ', 'just sign bil  our milit wil now be stronger than ev bef  we lov and nee our milit and gav them everyth   and mor  first tim thi has hap in a long tim  also mean job  job  job ', 'wow   sen mark warn got caught hav extend contact with a lobby for a russ oligarch  warn did not want a  pap trail  on a  priv  meet  in london  he request with steel of fraud dossy fam  al tied into crook hil ', 'tim to end the vis lottery  congress must sec the immigr system and protect am ', 'as long as we op our ey to god s grac   and op our heart to god s lov   then americ wil forev be the land of the fre  the hom of the brav  and a light unto al nat ', 'i wil be meet with henry kiss at     pm  wil be discuss nor kore  chin and the middl east ', 'our found invok our cre four tim in the decl of independ  our cur decl  in god we trust   and we plac our hand on our heart as we recit the pledg of allegy and proclaim that we ar  on nat und god  ', 'wil be head ov short to mak remark at the nat pray breakfast in washington  gre religy and polit lead  and many friend  includ t v  produc mark burnet of our wond    season appr triumph  wil be ther  look forward to see al ', 'the budget agr today is so import for our gre milit  it end the dang sequest and giv secret mat what he nee to keep americ gre  republ and democr must support our troop and support thi bil ', 'congrat to the republ of kore on what wil be a magn wint olymp  what the sou kor peopl hav built is tru an inspir ', 'best wish to the republ of kore on host the what a wond opportun to show everyon that you ar a tru gre nat ', 'new fbi text ar bombshel ', 'in the  old day   when good new was report  the stock market would go up  today  when good new is report  the stock market goe down  big mistak  and we hav so much good  gre  new about the econom ', 'congrat and on the success launch  thi achiev  along with commerc and intern partn  continu to show am ingenu at it best ', 'happy birthday to our   th presid of the unit stat of americ  ronald reag ', 'today  we heard the expery of law enforc profess and commun lead work to comb the threat of ms     and the reform we nee from congress to def it  watch her ', 'we nee a   st century merit bas immigr system  chain migr and the vis lottery ar outd program that hurt our econom and nat sec ', 'pol show near   in    am support an immigr reform pack that includ dac  ful sec the bord  end chain migr  amp  cancel the vis lottery  if d s oppos thi deal  they ar t sery about dac they just want op bord ', 'my pray and best wish ar with the famy of edwin jackson  a wond young man whos lif was so senseless tak ', 'so disgrac that a person illeg in our country kil lineback edwin jackson  thi is just on of many such prev tragedy  we must get the dem to get tough on the bord  and with illeg immigr  fast ', 'thank to the hist tax cut that i sign into law  yo paycheck ar going way up  yo tax ar going way down  and americ is ont again op for busy ', 'repres devin nun  a man of tremend cour and grit  may someday be recogn as a gre am hero for what he has expos and what he has had to end ', 'any deal on dac that doe not includ strong bord sec and the desp nee wal is a tot wast of tim  march  th is rapid approach and the dem seem not to car about dac  mak a deal ', 'littl adam schiff  who is desp to run for high off  is on of the biggest liar and leak in washington  right up ther with comey  warn  bren and clap  adam leav clos commit hear to illeg leak confid inform  must be stop ', 'thank you to for expos the tru  perhap that s why yo rat ar soooo much bet than yo untruth competit ', 'the democr ar push for univers healthc whil thousand of peopl ar march in the uk becaus their u system is going brok and not work  dem want to gre rais tax for real bad and non person med car  no thank ', 'congrat to the philadelph eagl on a gre sup bowl vict ', 'my thought and pray ar with al of the victim involv in thi morn train collid in sou carolin  thank you to our incred first respond for the work they ve don ', '   a tool of ant trump polit act  thi is unacceiv in a democr and ought to alarm anyon who want the fbi to be a nonpart enforc of the law    the fbi wasn t straight with congress  as it hid most of thes fact from investig   wal street journ', ' the four pag memo releas friday report the disturb fact about how the fbi and fis appear to hav been us to influ the      elect and it afterma    the fbi fail to inform the fis court that the clinton campaign had fund the dossy    the fbi becam    ', 'gre job numb and fin  aft many year  ris wag  and nobody ev talk about them  on russ  russ  russ  despit the fact that  aft a year of look  ther is no collud ', 'thi memo tot vind  trump  in prob  but the russ witch hunt goe on and on  their was no collud and ther was no obstruct  the word now us becaus  aft on year of look endless and find noth  collud is dead   thi is an am disgrac ', 'rasmuss just annount that my approv rat jump to      a far bet numb than i had in win the elect  and high than certain  sacr cow   oth trump pol ar way up also  so why doe the med refus to writ thi  oh wel  someday ', 'rt       ', 'rt       ', 'today  it was my hon to join the gre men and wom of and at the u s  custom and bord protect nat target cent in sterl  virgin  fact sheet ', ' trump the or outlin the gre of americ to democr  disgust ', 'with     mil am receiv bonus or oth benefit from their employ as a result of tax cut       is off to gre start   unemploy rat at        av earn up      in the last year           new am job ', ' you had hil clinton and the democr party try to hid the fact that they gav money to gps fus to cre a dossy which was us by their al in the obam admin to convint a court mislead  by al account  to spy on the trump team   tom fitton  jw', 'the top lead and investig of the fbi and the just depart hav polit the sacr investig process in fav of democr and against republ   someth which would hav been unthink just a short tim ago  rank  amp  fil ar gre peopl ', 'lov thi ', 'tun in today at     pm et      am pt ', 'a new book is out today that pick up wher i left off in what hap in explain  thos damn email   ter  fact fil read ', '   year ago  sign the famy and med leav act  which remain a success al thes year lat  now we nee paid leav to fin the job and cre mor equit  amp  famy friend workplac ', 'look forward to being a part of and talk about how we can al rais our voic  watch liv and wed     at     pm et      am pt  learn mor her ', 'tun in ', ' ', 'i wrot a facebook post about a decid i mad    year ago  what s chang   amp  on an issu you didn t hear a singl word about tonight  tak a look ', 'i cal her today to tel her how proud i am of her and to mak sur she know what al wom should  we deserv to be heard ', 'a story appear today about someth that hap in       i was dismay when it occur  but was heart the young wom cam forward  was heard  and had her concern tak sery and address ', 'thank you  for yo tireless advoc on behalf of wom and girl  and for yo grac und press ov thes last    year  and thank you to for al you ve don and continu to do to adv reproduc right  onward ', 'for thos of you who don t know she s a suprem tal young wom with a ter podcast  she is also cour  amp  grac man a cant diagnos  amin  send you good vib post surgery  amp  shar yo inspir thread ', 'in       the wom s march was a beacon of hop and defy  in       it is a testa to the pow and resy of wom everywh  let s show that sam pow in the vot boo thi year ', 'rt rep  john lew  benny thompson to attend grand celebr of mississipp civil right muse', 'i m so heart by al of you  onward ', 'thes word from dr  king also com to mind today ', 'beauty said  an import mess today and every day ', 'the annivers of the devast earthquak   year ago is a day to rememb the tragedy  hon the resy peopl of hait   amp  affirm americ s commit to help our neighb  instead  we re subject to trump s ign  rac view of anyon who doesn t look lik him ', 'nant has a record of beat the od   from dc to ca   wher she help gov brown do a fantast job  al who know her ar send strength  amp  lov as she fac thi latest challeng  onward my friend   h', 'rt two week ago a    year old soldy rac rep into a burn bronx apart build  sav four peopl bef he ', "rt today  the amaz the destruct of hil clinton is out in paperback  we'r celebr with thi exc ", 'famy across americ had to start      worry that their kid wouldn t hav heal car  fail to act now show the tru fac of republ  amp  their don driv im agend  you control the sen agend enough is enough ', 'tim to bring chip to the sen flo as prom  thi alleg extend until march doesn t cut it as stat freez enrol  amp  send out let warn that cov wil end  thi is fright to par  amp  wreak havoc for stat ', 'the ir peopl  espec the young  ar protest for the freedom and fut they deserv  i hop their govern respond peac and support their hop ', 'rt retweet if you agr it s tot crazy to suggest that the fbi   hav help sink hil s campaign by rev that she ', 'rt must read  form clinton deputy  how rex tillerson can right the stat depart ', 'thank you to everyon who has don to onward togeth in our first year    we re on abl to support thes gre group becaus of you  let s do ev mor in       onward ', "along with and i know thes group wil continu to do the incred work of mak our democr stronger in       and i'm proud to be on their team ", 'and with the help of    afr am candid hav been elect to loc  stat  and fed off sint august      ', 'mor than     stud and lead met last mon at the pow summit to shar resourc and tool for nat advoc ', 'the team at is work to elect mor latino candid at every level of govern ', 'at young peopl ar us loc org to address econom ineq  attack on vot right  and mor ', "    brand new civ lead and act attend the very first    and they're on track to train       peopl by the end of      ", 'the org at work with partn on the ground to get autom vot reg on the ballot in nevad ', 'onward togeth is end      by support six mor incred org fight to protect vot right and to mak it easy for young  divers candid to get on the ballot and get elect ', 'rt what hap by was nam a best book of the year by     the new york tim    the washington post    npr ', 'someth produc to do with yo out today ', 'i guest edit thi mon s issu of it was a wond expery  with lot of ter contribut from peopl i lov  amp  respect  read thi let that my daught wrot to her childr about why she s stil optim about the fut  cc ', 'a littl girl pow in pasaden   ', 'som photo from the book tour  i met so many incred peopl  includ som littl hero   amp  the occas superhero   ', 'return hom aft the fin week of the book tour for       thi was the last cop of what hap i sign at our fin sign in seattl  what a ter journey thi has been so far  thank you to al who cam out  amp  told me yo story ', 'thank you to the  amp  for a ter discuss  a wond group of young peopl   amp  a tru inspir day ', 'today is the fin day to sign up for      heal cov  go to and get cov ', 'think of you  and al the sandy hook famy thi week  thank you for yo cour ', 'ye  elect mat ', "tonight  alabam vot elect a sen who'll mak them proud  and if democr can win in alabam  we can    and must    compet everywh  onward ", 'may ed lee s dea is a terr loss for the peopl of san francisco  he was a good friend  amp  a voc advoc for the city he serv  amp  lov  my thought ar with his famy ', "if you don't liv in an are that could be affect  read thi to see how you can help thos who do ", "think about al affect  amp  thos who could be affect by the californ wildfir  if you liv near a fir  mak sur you're prep   amp  stay saf ", 'i m going to keep tweet about thi  and speak out every chant i get  until it is fix ', 'dr  king was    when the montgomery bus boycot beg  he start smal  ral oth who believ their effort mat  press on through challeng and doubt to chang our world for the bet  a perm inspir for the rest of us to keep push toward just ', 'al across americ peopl chos to get involv  get eng and stand up  each of us can mak a diff  and al of us ought to try  so go keep chang the world in      ', 'ten year old jahkil jackson is on a miss to help homeless peopl in chicago  he cre kit ful of sock  toiletry  and food for thos in nee  just thi week  jahkil reach his goal to giv away        bless bag   that s a story from      ', 'chris long gav his paycheck from the first six gam of the nfl season to fund scholarships in charlottesvil  va  he want to do mor  so he decid to giv away an entir season s sal  that s a story from      ', 'kat creech  a wed plan in houston  turn a postpon wed into a volunt opportun for hur harvey victim  thirty wed guest becam an org of hundr of volunt  that s a story from      ', "as we count down to the new year  we get to reflect and prep for what s ahead  for al the bad new that seem to domin our collect conscy  ther ar countless story from thi year that remind us what's best about americ ", 'rt i am my broth s keep  watch our new psa with  amp  then tak act to s ', 'on behalf of the obam famy  merry christmas  we wish you joy and peac thi holiday season ', "there's no bet tim than the holiday season to reach out and giv back to our commun  gre to hear from young peopl at the boy  amp  girl club in dc today ", 'happy hanukkah  everybody  from the obam famy to yo  chag sameach ', "just got off a cal to thank folk who ar work hard to help mor am across the country sign up for heal cov  but it's up to al of us to help spread the word  sign up through thi friday at", 'rt watch  we host a town hal in new delh with and young lead about how to driv chang and mak an im ', 'michel and i ar delight to congrat print harry and megh markl on their eng  we wish you a lifetim of joy and happy togeth ', 'from the obam famy to yo  we wish you a happy thanksg ful of joy and gratitud ', 'me  joe  about halfway through the speech  i m gonn wish you a happy bir   bid  it s my birthday  me  joe  happy birthday to my broth and the best vic presid anybody could hav ', 'rt today  we hon thos who hav hon our country with it highest form of serv ', "thi is what hap when the peopl vot  congr and   and congrat to al the vict in stat legisl  county and mayors' rac  every off in a democr count ", 'every elect mat   thos who show up determin our fut  go vot tomorrow ', 'may god also grant al of us the wisdom to ask what concret step we can tak to reduc the viol and weaponry in our midst ', 'we griev with al the famy in sutherland springs harm by thi act of hat  and we ll stand with the surv as they recov   ', 'start today  you can sign up for      heal cov  head on ov to and find a plan that meet yo nee ', "michel and i ar think of the victim of today's attack in nyc and everyon who keep us saf  new york ar as tough as they com ", 'hello  thrilled to host civ lead in chicago from al ov the world  follow along at', 'i ll let you and handl the sing  and we ll handl the don  ther s stil tim to giv ', 'tonight the ex presid ar get togeth in texa to support al our fellow am rebuild from thi year s hur  join us ', "i'm grat to for his lifetim of serv to our country  congrat  john  on receiv thi year's liberty med ", 'michel  amp  i ar pray for the victim in las vega  our thought ar with their famy  amp  everyon end anoth senseless tragedy ', 'proud to che on team us at the invict gam today with my friend joe  you repres the best of our country ', "we'r expand our effort to help puerto rico  amp  the usv  wher our fellow am nee us right now  join us at", 'prosecut  soldy  famy man  cit  beau mad us want to be bet  what a leg to leav  what a testa to', 'rt presid address start at       pm  tun in her ', 'think about our neighb in mexico and al our mex am friend tonight  cuidens mucho y un fuert abrazo par todo ', 'cod is import   and fun  thank for yo work to mak sur every kid can compet in a high tech  glob econom ', "michel and i want the to inspir and empow peopl to chang the world  here's how we'r get start thi fal ", 'we rememb everyon we lost on      and hon al who defend our country and our id  no act of ter wil ev chang who we ar ', 'rt across the u s   am hav answ the cal to help with hur recovery  pray for al florid ', 'proud of thes mckinley tech stud inspir young mind that mak me hop about our fut ', 'am alway answ the cal ', 'to target hop young strivers who grew up her is wrong  becaus they ve don noth wrong  my stat ', "thank you to al the first respond and peopl help each oth out  that's what we do as am  here's on way you can help now ", 'michel and i ar think of the victim and their famy in barcelon  am wil alway stand with our span friend  un abrazo ', '    for lov com mor nat to the hum heart than it opposit     nelson mandel', ' peopl must learn to hat  and if they can learn to hat  they can be taught to lov    ', ' no on is born hat anoth person becaus of the col of his skin or his background or his relig    ', "john mccain is an am hero  amp  on of the bravest fight i've ev known  cant doesn't know what it's up against  giv it hel  john ", "heal car has alway been about someth big than polit  it's about the charact of our country ", "of al that i've don in my lif  i'm most proud to be sash and malia's dad  to al thos lucky enough to be a dad  happy father's day ", 'on thi nat gun viol aw day  let yo voic be heard and show yo commit to reduc gun viol ', 'forev grat for the serv and sacr of al who fought to protect our freedom and defend thi country we lov ', 'good to see my friend print harry in london to discuss the work of our found  amp  off condol to victim of the manchest attack ', 'i m also in the abbey road photo if you look clos ', 'jerry seinfeld hold a baby pug at the grammy wil mak yo week i m just her becaus my alb   stand up comedy to mak lov to  was nomin ', '  wow  get som diff mat ', 'thi piec brought me clos to heav  thank alex  saw doc pom at the comedy club so much ', 'rt let of recommend  rodney dangerfield', 'trudeau turn to seinfeld tact to tam town hal heckl try throwing pap towel into the crowd ', 'don t miss my favorit show about a man and his mou ', 'jerry seinfeld at israel s ramon airbas with the israel air forc bomb  world of diff between them and me ', 'rt gre new  com in car get coff just went on netflix  you can see chris rock  amp  oth pal of jerry hav ', 'jerry seinfeld spot at tel av s best falafel shop  just nee someth to hold me ov until my din falafel ', "rt ar you a fan of com  car  coff  try not to freak out  but we'v got som new  season     ar now streaming  ", 'jerry seinfeld convint hugh jackm to retir as wolverin can t wait for thi post  com soon  jerry seinfeld as wolverin ', ' bee movy    celebr    year and ov   mil   thi is kind of weird   com mad ', 'rt nev thought i would be ment in the sam tweet as  ', 'ok  i ll  friend  you ', 'i m pick bridget everet s new sitcom as my favorit new  adult  comedy i ve seen thi year  so new  diff and funny  don t miss   lov you mor  wednesday night on amazon prim ', 'rt tough crowd  tak on', 'rt ther  we said it ', "as i've said many tim  if you don't get jerry lew  you don t understand comedy  spend an afternoon with   ", 'me and my man of three decad  enjoy melbourn ', 'ted l  nant read his let liv  includ shout out  la class  op jun   ', 'hil ', "debut of podcast  we cov automot psychos  hitler's     r  bonhom   amp  the hug thing ", "thank again to for yesterday's fath lunch  her s an interview i did with them ", "alright  i'm her in worcest for my show tonight and nobody her can ev agr on how the hel you say it  not giv up    ", "i cannot understand the spel and pronunt of worcest  mass  so i'm com in for   show saturday night to fig it out ", "i'm return to montreal's the first tim in ov    year with on      ", "there's a beauty new stag i lov it  thank you   let's do it again tonight   p ", "going to do a drop in set in about an hour   av   st  if you're in the neighb  ", "grey swe solid for my wife's new book   food swing   out today everywh  amp  already bestsel ", 'a smal but very import plac in my lif can now be an import but stil smal plac in yo lif ', 'meet me on tour     front row tix to my nyc show  bid on  amp  process wil benefit', 'thi plac ', "going to work out som stuff tonight in burbank  if you want to see what that's lik  ", '     mor class ', 'go see thi weekend i was just ther tonight  he destroy me  unbeliev funny ', 'new  com in car get coff  bob einstein is back  enjoy a second cup ', 'new  com in car get coff  christoph waltz  an eleg refin man  join us at ihop ', "pretty obvy the bas structure of my show was bas on the mtm show  hub of the wheel  let's say  but i ad her  perfect ", 'new  com in car get coff lew black  black s lif mat ', 'new com car get coff  cedr the entertain  no affy with cedr the regul person ', 'new episod today  com in car get coff norm macdonald  thi comedy standard is the norm ', 'season   premy   krist wiig  la  noth to do  two plac to get it don  cream ', "i do lov al the festiv nonsens every year  holiday ar nic but som sil was good to ad  thank again to dan o'keefe ", 'new singl shot   i want my old tv   the re memb on club ', 'lov thi and jerry l  so much  the ess of every com on display  wish thi was min ', 'new singl shot  is thi  a bit     men work  our motto  saf is numb    funny is numb   ', 'new singl shot lov thi car hat thi car  it s not wher you re going  it s if you can ev get ther ', 'new singl shot  coff rebel   com in car not drink coff ', 'rt i just earn my phd in new york city hist  and i found out i speak dutch from watch with', '  ', 'i grew up watch and now i get to voic myself in an episod     stay tun   ', 'thi post is for the incred so much to say about thi incred wom  not on an admir storytel  she is a wond lead  not afraid to  lit  get into the trench  lucky to cal her a collab  friend  sist in film ', 'i m join to fight for the dream of girl everywh  join me  amp  at ', 'ready for the weekend  ', 'tak a dant break at my thank and met towley for teach me the ', 'bts of the photoshoot with', 'today is holocaust remembr day  a day to hon the holocaust victim  may we nev forget ', 'thank you so much and for the beauty photo with the most amaz and tal company ', '  ', 'you ar wond wom  and of cours i wil    ', 'what a wond tim at the pga award last night ', 'unit we stand ', ' ', 'girl gon wild    when patty and i tak charad sery  ', '', 'tim to relax   ', 'i just hav to say thank you so so much to the team of the for hon me with the award last night  and also a hug thank you to the crit for hon as best act film ', '       ', 'thank you to the crit for recogn wond wom as best act movy  ', 'wow  i just watch thi whol video and i m in tear  so inspir and  ', 'thank you for the wond night and for hon patty and i with the spotlight award ', 'you know i lov a bold red lip  right  wel  i m about to mak it off  check out thi littl teas from', ' so i want al the girl watch her  now  to know that a new day is on the horizon    ', 'so happy to annount that i ve join the famy and am help launch the campaign  stay tun it s going to be beauty ', 'such an amaz night  surround by tal  inspir  and good peopl    ', 'thank you  ', 'wow  it s been an amaz year   excit for the tonight ', 'thank you   ', '', 'thank you palm springs intern film fest for the amaz night and hon me with the ris star award  big congr to al the oth hon of the night as wel ', 'i sign thi let of solid to stand with wom and men across every industry who hav expery sex harass  assault  or abus in say  enough is enough', "thank you      you've been incred       i'm ready for you     wish you al   happy  heal and luck   happy new year   ", 'i might hav been a littl mor excit than everyon els  ', 'look forward to the new year  ', 'happy holiday to al of you  may we alway be grat and smil  ', 'in such good company  thank you', 'feel very grat  thank you and for thi hon ', 'nev was i so happy on a monday  ready to start the holiday     ', 'thursday got me lik    ', 'thank you so much for the acknowledg and the kind word you wrot about us ', 'thank for ev lik thi so i can meet al of you      ', 'diff is spec  you re beauty keaton  insid and out ', 'no thank you  i couldn t hav don thi without you captain  lov you to the themyscir and back   ', 'hey ell  you re it   join the gam ', 'had such a wond tim at the gq men  wom  of the year party  thank you for hav me ', '  ', 'it was such an hon to be abl pres the first ev wond wom scholarship yesterday to thi wond wom  carl at the wom in entertain ev ', 'wow   year ago   and forev grat to be abl to play thi charact ', 'relax   ']
    

# PART 2

now we will use COuntVectorizor in order to create the bag of words, we will set the stop_words parameter to the stopword
in corpus in order to remove all of them for better classification, We also set the max_features to 1000 because we wanted the bag to be in medium large, that we will get good result, this size is according to the vocabulary size and tokenizing we will declare later.


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
    
    

Now we will divide the input into train and test, in order to run the models on the train to fit them and on the test to find the model accuracy.


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




    0.5370370370370371



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




    0.14814814814814814



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




    0.5370370370370371



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




    0.6296296296296297



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




    0.6481481481481481



now we will create a diagram that shows whice algorithm as the better result, and the result of each one from the 5 algorithm.


```python
import matplotlib.pyplot as plt
import plotly.plotly as py

dictionary = plt.figure()

D = {u'Random_Forest':random_forest, u'KNN': k_near_neighbur, u'SVC':support_vector , u'Regression':regression_logistic, u'Naive_Bayes':naive_bayes}

plt.bar(range(len(D)), D.values(), align='center')
plt.xticks(range(len(D)), D.keys())

plt.show()
```


![Image of github's cat](/images/photo1.png)


Here we could see that for the last 50 tweets of each person, the Naive_Bayes score is the best score

# part 3

# Deep Learning

install the keras library in order to create tweets for each of our 5 famous.


```python
!pip install keras==1.2
```

TensorFlow is also supported (as an alternative to Theano), but we stick with Theano to keep it simple. The main difference is that we will need to reshape the data slightly differently before feeding it to our network.


```python
from keras import backend as K
K.set_image_dim_ordering('th')
```

    Using TensorFlow backend.
    C:\Anaconda\lib\site-packages\h5py\__init__.py:34: FutureWarning:
    
    Conversion of the second argument of issubdtype from `float` to `np.floating` is deprecated. In future, it will be treated as `np.float64 == np.dtype(float).type`.
    
    

We will take all trumps tweets and make them a single sequence for each of our famous, every tweet in the sequences we will separate by the delimiter NEWTWEET, in order to know when to stop the creation of every tweet in the future.


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

Now we want to remove all hastag and tags (@, #) , and all the http words from the tweets.and get for each famous new sequence of tweets after cleaning.


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




    "📸📸 NEWTWEET I grew up watching and now I get to voice myself in an episode! 😱😜 Stay tuned... NEWTWEET This post is for the incredible so much to say about this incredible woman. Not only an admirable storyteller. She is a wonderful leader, not afraid to (literally) get into the trenches. Lucky to call her a collaborator, friend, sister in film. NEWTWEET I’m joining to fight for the dreams of girls everywhere. Join me &amp; at: NEWTWEET Ready for the Weekend 😎 NEWTWEET Taking a dance break at my Thanks and Mette Towley for teaching me the🍋 NEWTWEET BTS of the photoshoot with NEWTWEET Today is Holocaust Remembrance Day. A day to honor the Holocaust Victims. May we never forget. NEWTWEET Thank you so much and for the beautiful photo with the most amazing and talented company! NEWTWEET 🌊🌊 NEWTWEET You ARE Wonder Woman. And of course i will! ❤️ NEWTWEET What a wonderful time at the PGA Awards last night. NEWTWEET United We Stand! NEWTWEET 🕶 NEWTWEET Girls gone wild..? When Patty and I take charades seriously 😅 NEWTWEET NEWTWEET Time to relax... NEWTWEET I just have to say thank you so so much to the team of the for honoring me with the award last night! And also a huge thank you to the critics for honoring as Best Action Film. NEWTWEET 🙅🏻\u200d♀️💃🏻 NEWTWEET Thank you to the Critics for recognizing Wonder Woman as best action movie!! NEWTWEET Wow! I just watched this whole video and I’m in tears. So inspirational and ! NEWTWEET Thank you for the wonderful night and for honoring Patty and I with the spotlight award. NEWTWEET You know I love a bold red lip, right? Well, I’m about to make it official. Check out this little teaser from NEWTWEET “So I want all the girls watching here, now, to know that a new day is on the horizon!” - NEWTWEET So happy to announce that I’ve joined the family and am helping launch the campaign. Stay tuned…it’s going to be beautiful! NEWTWEET Such an amazing night. Surrounded by talented, inspiring, and good people! ✌️ NEWTWEET Thank you ! NEWTWEET Wow. It’s been an amazing year!! Excited for the tonight! NEWTWEET Thank you ❤️ NEWTWEET NEWTWEET Thank you Palm Springs International Film Festival for the amazing night and honoring me with the Rising Star Award. Big congrats to all the other honorees of the night as well. NEWTWEET I signed this letter of solidarity to stand with women and men across every industry who have experienced sexual harassment, assault, or abuse in saying: enough is enough NEWTWEET Thank you 2017 you've been incredible. 2018 I'm ready for you.. 😏 wishing you all - happiness, health and luck!! Happy New Year!!! NEWTWEET I might have been a little more excited than everyone else 😂 NEWTWEET Looking forward to the New Year.. NEWTWEET Happy Holiday to all of you! May we always be grateful and smile 😊 NEWTWEET In such good company. Thank you NEWTWEET Feeling very grateful! Thank you and for this honor. NEWTWEET Never was I so happy on a Monday- ready to start the holidays 🤘🏼\U0001f91f🏼 NEWTWEET Thursday got me like.... NEWTWEET Thank you so much for the acknowledgment and the kind words you wrote about us! NEWTWEET Thankful for events like this so I can meet all of you! 🙅🏻\u200d♀ NEWTWEET Different is special. You’re beautiful Keaton. Inside and out. NEWTWEET No thank YOU! I couldn’t have done this without you Captain! Love you to the themyscira and back! 😘 NEWTWEET Hey Ella, you’re it! 💕Join the game! NEWTWEET Had such a wonderful time at the GQ Men (Woman) of the Year Party. Thank you for Having me! NEWTWEET 😱😃 NEWTWEET It was such an honor to be able present the first ever Wonder Woman scholarship yesterday to this WONDERful woman, Carla at the Women In Entertainment Event. NEWTWEET Wow 4 years ago?! And forever grateful to be able to play this character! NEWTWEET Relaxing... NEWTWEET Thank you so much. Hope all is well! 😘 NEWTWEET Gisele NEWTWEET I really got the chills hearing them.. is this real ? NEWTWEET So excited to meet you all! Who is all coming? NEWTWEET Thank you for this sit down with the very funny ! NEWTWEET Wow this is amazing! And I couldn’t ask for anyone else to receive this award with! thank you 🙏🏻❤️ NEWTWEET My partner in crime NEWTWEET Thank you E! News! So sweet! NEWTWEET NEWTWEET Thank you so much ! You’re the best! NEWTWEET Brooklynn, you are such a bright ray of light! So talented! I saw every bit of Wonder Woman in you when we met! 🙅🏻 NEWTWEET It was wonderful speaking with Willie Geist for NEWTWEET Thank you, thank you for your support! I’ve been reading your posts and seeing the photos in the theaters, and ticket stubs! You are the best fans! NEWTWEET RT Behind-the-scenes with GQ cover star NEWTWEET Bright and Early with the talking Justice League. Thanks for having me! NEWTWEET Such an honor!! Thank you so much ! NEWTWEET It might be unrealistic but my weekend goals! 😴 NEWTWEET This is incredible!!! So grateful for working w/ such amazing ppl &amp; for your amazing reviews 🙏🏻🙏🏻 NEWTWEET Last time we were all in London together we were filming Justice League. Can’t believe the time is here! NEWTWEET What is she is doing with my tiara?? NEWTWEET Press Tour Stop starts today in London! 🙅🏻💃🏻 NEWTWEET It was such an honor to be able to stand with these real life Wonder Woman. Thank you for letting me share this moment with you all! NEWTWEET Thank you for this amazing opportunity! NEWTWEET 😂😂 NEWTWEET 🙏🏻love your work! NEWTWEET The Tour is off to an amazing start! Thank you China for having us! 🙅🏻💃🏻🇨🇳 NEWTWEET China NEWTWEET 🙅🏻🙅🏻 NEWTWEET Very much noted! She’s a real life hero! Im sending her a huge hug of love, good energy and strength. NEWTWEET RT An amazing event for a very personal cause. My wonderful daughter Willow courageously leads our family as we walk to raise… NEWTWEET I’m such a big fan . You were so sweet today thanks for the awesome words! NEWTWEET NEWTWEET NEWTWEET It’s finally here! Tonight I’m hosting with musical guest Sam Smith ! Make sure you tune in! 💃🏻 Photos by Mary Ellen Matthews NEWTWEET RT Fun show tonight: is here, stop by, performs &amp; more! NEWTWEET Beyond excited to be hosting this Saturday with musical guest . Make sure you tune in! 💃🏻 NEWTWEET UK friends! You can get your hands on on Blu-ray on October 9! Patty wasn’t exaggerating when she said it was freezing cold.❄️ NEWTWEET RT NEW with &amp; more... NEWTWEET NEWTWEET No longer a secret, so excited to be hosting NEWTWEET Shana Tova to all of you! May this year be filled with joy , happiness, creation and love!! NEWTWEET Today is the day! is now available on Blu-ray™ I can't thank everyone enough for the love and support for this movie!🙅🏻 NEWTWEET What an amazing ride this has been! See the rest of the outtakes on the DVD extras 9/19. NEWTWEET 300 drones lit up the LA sky tonight to celebrate Power, Grace, Wisdom, and Wonder. Bring home on Digital Now and Blu-ray™ 9/19 NEWTWEET So well deserved my sister. Couldn't be happier for you. Paving the right way for so many to come NEWTWEET Thank you Rachel for the kind words about Wonder Woman! I'm a big fan of yours! Xo NEWTWEET There truly is a little Wonder in ALL of us! NEWTWEET Chris's nickname for me was giggle Gadot, &amp; I could communicate with no words. I enjoyed filming every second of the movie.💃🏻🙅🏻🎬 NEWTWEET Morgan and Meg are the perfect example of real life superheroes. Sending my strength and positive energy to both of you! NEWTWEET Just a taste of the fun we had while filming, more to come with the DVD's extras, on 9/19! We weren't always so serious on Themyscira. 😜💃🏽🙅🏻 NEWTWEET Looking amazing ladies! ❤️💃🏽💪🏻 NEWTWEET RT The Warrior Princess has arrived. Own on Digital Today! NEWTWEET It was an honor to present the Video of the Year award tonight at the ! Congrats to and all the other nominees. NEWTWEET RT NEWTWEET Wonder Woman was released in Japan! I hope you enjoy! ❤️ NEWTWEET Such an honor. Thank you ! NEWTWEET So true. NEWTWEET Wow! Just heard the news! Thank u to everyone who has shown their support to WW in theaters! What an amazing ride this has been! NEWTWEET Chasing Waterfalls 🌈🏞 NEWTWEET Sending all my love to Barcelona and those affected by this horrible tragedy NEWTWEET Thank you for the support of Wonder Woman last night, 3 awards- wow! We are blessed with fans like you! NEWTWEET Exciting news! is coming to Digital 8/29 and Blu-ray™ 9/19. NEWTWEET Another day at work. is raving &amp; is working &amp; I'm just trying to look cool. Art Direction: NEWTWEET I hope you enjoyed watching the NEWTWEET to standing on the blue carpet with our fearless leader, Patty Jenkins, for the premiere. NEWTWEET Amazing article – thank you 👏😘 NEWTWEET You can't save the world alone ✨❤️ from NEWTWEET from way for justice. Enjoy this sneak peak for now 🙏🏻 In theaters November 17. NEWTWEET Ready or not here we come...🤘🏼😏😆 benaffleck NEWTWEET These women are so inspirational! Love seeing them making an impact in the world of game design. NEWTWEET to last year at ❤️ Can’t wait to see you all this weekend 😉 NEWTWEET I love stumbling across photos from 💖💥 NEWTWEET It's the little things that make a difference 🙌 NEWTWEET Well said 🙏🏻💕 NEWTWEET This is incredible! 🙌 Thanks to all of you for making this possible 😀😘 NEWTWEET 🌸🌸 NEWTWEET Black and white glamour 😉💋 NEWTWEET unite 💥🙏🏻 from ✨ NEWTWEET When it's finally Friday... 🙌🏻😉 NEWTWEET Thanks to ALL of you for making a success. Wonder Woman has the best fans in the world ❤️ thx NEWTWEET Chocolate is always the answer 😉🙏 NEWTWEET NEWTWEET you're the best! Thank you for this.. love you and so much! 💋❤⭐️🤘🏼 NEWTWEET Wow 🙏 A huge thank u to all for making a success ❤️ thx for this piece NEWTWEET Getting thrown into Monday like... 😂 NEWTWEET 🖤🖤 NEWTWEET We had fun ❤️ Here is some more exclusive from ⚔️🙏🏻Enjoy ✨ NEWTWEET Kisses from my Lola 🐶😘 NEWTWEET NEWTWEET 💪🙏 NEWTWEET The smiling skies 😀 NEWTWEET Hope everyone is having a wonderful shared with family and friends 🙏🏻🙏🏻 NEWTWEET Hello, July 💋☀️ NEWTWEET Weekend got me like... 🙌🏻 NEWTWEET Always laughing with these two 😘😋 NEWTWEET NEWTWEET Another from 🙏 ❤️ NEWTWEET Relaxing and enjoying some scenery ❤️🌴 NEWTWEET 🖤 ⚔️ NEWTWEET So excited I finally get to share with you, Spain ✨⚔️🙏 disfrutar ❤️ NEWTWEET from 😘 NEWTWEET to this day ❤️ NEWTWEET Peek a boo 🙈🤗 NEWTWEET Happiness ☀️❤️ NEWTWEET 🖤 NEWTWEET Me + you = forever ❤️ NEWTWEET from Here’s some BTS footage from NEWTWEET To my other half , father of my children and love of my life . You are simply the… NEWTWEET Trying to hypnotize you... NEWTWEET Sleepless night , colic 3 months old baby and an early wake up by my 5 year old. Went to the… NEWTWEET Wow! Thx so much NEWTWEET It was always such a joy working with my family ❤️⚔️ We had a blast 💥🙌🏻 NEWTWEET And one more photo from ✨ NEWTWEET Wow! This is incredible 🙏👏 Bravo NEWTWEET Make sure to always eat your greens 💚 NEWTWEET This is awesome! 🙏 NEWTWEET Wow the last paragraph really gave me the chills. So true. So powerful. Gives me a huge drive to dive in and work on the next one..🙏🏻 NEWTWEET Pure joy working on bc I was surrounded by great people❤️ NEWTWEET Thank you for having me Grazia China 🇨🇳 NEWTWEET RT It's official! has become the most Tweeted movie of 2017. Congrats to and the cas… NEWTWEET Can’t thank you all enough for making 💥⚔🙏 NEWTWEET 💜💙🙏 thx for this cool piece 💋 NEWTWEET To my fans. Thank you all, I love you all so much 🙏🏻😘💋 תודה, धन्यवाद, Merci, Danke, 谢谢, Gracias, ありがと, Obrigado ❤️ NEWTWEET What was your favorite part of costume? ✨🙏 from NEWTWEET from 💋I thought she knew me 😩😜🍸 NEWTWEET Thanks so much my friend 😘🙏🏻 NEWTWEET Take that 😉💥✨ NEWTWEET 🙏🏻🙏🏻🙏🏻 NEWTWEET Wow. Thank you so much. Means a lot coming from you 🙏🏻 NEWTWEET 🙏🏻🍀😘 NEWTWEET Thank you so much James, so happy you enjoyed it. Can't wait for Aquaman!!! NEWTWEET So happy you liked it Josh 💋 NEWTWEET Wow thank you so much 🙏🏻 sending you a kiss back!! NEWTWEET WOW! This is so moving and inspiring! NEWTWEET LOL, thank you so much! c u soon 😉 NEWTWEET Looking good girl! NEWTWEET It looks so good when you do it 🙅🏻🙅🏻 NEWTWEET The cutest!😍 NEWTWEET I always knew you were a smart guy :) But I think its worth a fight . we should collide worlds😏 NEWTWEET Wow thank you sister. I'm a big fan of your work and I'm so happy you enjoyed it 🙏 NEWTWEET .Thank you so much for the love 🙏 Miss you big guy!! 😘 NEWTWEET Can't believe how much fun and a joy it was making this movie. Truly blessed to work with these amazing people. NEWTWEET Hello players and fans, I am so happy to be the spokesperson for League of Angels - Paradise Land available now for all of you to play! NEWTWEET TODAY is the day! 🙌 ✨ is officially in theaters everywhere. Much love to all 💥⚔️ NEWTWEET Its almost time ⚔️💥 So thrilled to share with you all the latest poster for 🖤✨ NEWTWEET Add a bit more Wonder to your text messages with the new stickers ✨💙 Stickers available for iOS and Andriod download now ❤️ NEWTWEET Now this is a super squad ✨😉🙌 Thx ❤️ in theaters tomorrow! NEWTWEET Thx for this feature ❤️ Im honored to work alongside someone who I admire. Love u, 💋💥 NEWTWEET Loved having you by my side ✨❤️😘 Muah 💋 NEWTWEET I'm so excited to share my cover with you all. Thx so much 💋❤️ 📸 Photo taken by"



Because tweets are not very long we would like to keep only the most frequent words,
so we will set the vocabulary length to 600 because for the other words we don’t have a lot of contextual examples, 
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

Let's convert these special characters into the mentioned tokens, .... and ... we will convert to one .
We do this because in many times the tweet is not and by ... or .... so we need to keep generating the new tweet.


```python
def replace_specific_symbols_and_clean(text):
    text = text.replace('...', '.')
    text = text.replace('....', '.')
    text = text.replace('\n',' '+ line_break + ' ')
    text = text.replace('--',' '+ separator + ' ')
    text = text.replace('.',' '+sentence_end_token +' '+ sentence_start_token+' ' )
    return clean_text(text)
```

Now we will use the function above for every one of our famous tweets.


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




    "   NEWTWEET I grew up watching and now I get to voice myself in an episode     Stay tuned SENTENCEEND SENTENCESTART NEWTWEET This post is for the incredible so much to say about this incredible woman SENTENCEEND SENTENCESTART Not only an admirable storyteller SENTENCEEND SENTENCESTART She is a wonderful leader  not afraid to  literally  get into the trenches SENTENCEEND SENTENCESTART Lucky to call her a collaborator  friend  sister in film SENTENCEEND SENTENCESTART NEWTWEET I m joining to fight for the dreams of girls everywhere SENTENCEEND SENTENCESTART Join me  amp  at  NEWTWEET Ready for the Weekend   NEWTWEET Taking a dance break at my Thanks and Mette Towley for teaching me the  NEWTWEET BTS of the photoshoot with NEWTWEET Today is Holocaust Remembrance Day SENTENCEEND SENTENCESTART A day to honor the Holocaust Victims SENTENCEEND SENTENCESTART May we never forget SENTENCEEND SENTENCESTART NEWTWEET Thank you so much and for the beautiful photo with the most amazing and talented company  NEWTWEET    NEWTWEET You ARE Wonder Woman SENTENCEEND SENTENCESTART And of course i will     NEWTWEET What a wonderful time at the PGA Awards last night SENTENCEEND SENTENCESTART NEWTWEET United We Stand  NEWTWEET   NEWTWEET Girls gone wild SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART   When Patty and I take charades seriously   NEWTWEET NEWTWEET Time to relax SENTENCEEND SENTENCESTART NEWTWEET I just have to say thank you so so much to the team of the for honoring me with the award last night  And also a huge thank you to the critics for honoring as Best Action Film SENTENCEEND SENTENCESTART NEWTWEET         NEWTWEET Thank you to the Critics for recognizing Wonder Woman as best action movie   NEWTWEET Wow  I just watched this whole video and I m in tears SENTENCEEND SENTENCESTART So inspirational and   NEWTWEET Thank you for the wonderful night and for honoring Patty and I with the spotlight award SENTENCEEND SENTENCESTART NEWTWEET You know I love a bold red lip  right  Well  I m about to make it official SENTENCEEND SENTENCESTART Check out this little teaser from NEWTWEET  So I want all the girls watching here  now  to know that a new day is on the horizon     NEWTWEET So happy to announce that I ve joined the family and am helping launch the campaign SENTENCEEND SENTENCESTART Stay tuned it s going to be beautiful  NEWTWEET Such an amazing night SENTENCEEND SENTENCESTART Surrounded by talented  inspiring  and good people     NEWTWEET Thank you   NEWTWEET Wow SENTENCEEND SENTENCESTART It s been an amazing year   Excited for the tonight  NEWTWEET Thank you    NEWTWEET NEWTWEET Thank you Palm Springs International Film Festival for the amazing night and honoring me with the Rising Star Award SENTENCEEND SENTENCESTART Big congrats to all the other honorees of the night as well SENTENCEEND SENTENCESTART NEWTWEET I signed this letter of solidarity to stand with women and men across every industry who have experienced sexual harassment  assault  or abuse in saying  enough is enough NEWTWEET Thank you      you've been incredible SENTENCEEND SENTENCESTART      I'm ready for you SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART   wishing you all   happiness  health and luck   Happy New Year    NEWTWEET I might have been a little more excited than everyone else   NEWTWEET Looking forward to the New Year SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART NEWTWEET Happy Holiday to all of you  May we always be grateful and smile   NEWTWEET In such good company SENTENCEEND SENTENCESTART Thank you NEWTWEET Feeling very grateful  Thank you and for this honor SENTENCEEND SENTENCESTART NEWTWEET Never was I so happy on a Monday  ready to start the holidays      NEWTWEET Thursday got me like SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART NEWTWEET Thank you so much for the acknowledgment and the kind words you wrote about us  NEWTWEET Thankful for events like this so I can meet all of you       NEWTWEET Different is special SENTENCEEND SENTENCESTART You re beautiful Keaton SENTENCEEND SENTENCESTART Inside and out SENTENCEEND SENTENCESTART NEWTWEET No thank YOU  I couldn t have done this without you Captain  Love you to the themyscira and back    NEWTWEET Hey Ella  you re it   Join the game  NEWTWEET Had such a wonderful time at the GQ Men  Woman  of the Year Party SENTENCEEND SENTENCESTART Thank you for Having me  NEWTWEET    NEWTWEET It was such an honor to be able present the first ever Wonder Woman scholarship yesterday to this WONDERful woman  Carla at the Women In Entertainment Event SENTENCEEND SENTENCESTART NEWTWEET Wow   years ago   And forever grateful to be able to play this character  NEWTWEET Relaxing SENTENCEEND SENTENCESTART NEWTWEET Thank you so much SENTENCEEND SENTENCESTART Hope all is well    NEWTWEET Gisele NEWTWEET I really got the chills hearing them SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART is this real   NEWTWEET So excited to meet you all  Who is all coming  NEWTWEET Thank you for this sit down with the very funny   NEWTWEET Wow this is amazing  And I couldn t ask for anyone else to receive this award with  thank you      NEWTWEET My partner in crime NEWTWEET Thank you E  News  So sweet  NEWTWEET NEWTWEET Thank you so much   You re the best  NEWTWEET Brooklynn  you are such a bright ray of light  So talented  I saw every bit of Wonder Woman in you when we met     NEWTWEET It was wonderful speaking with Willie Geist for NEWTWEET Thank you  thank you for your support  I ve been reading your posts and seeing the photos in the theaters  and ticket stubs  You are the best fans  NEWTWEET RT Behind the scenes with GQ cover star NEWTWEET Bright and Early with the talking Justice League SENTENCEEND SENTENCESTART Thanks for having me  NEWTWEET Such an honor   Thank you so much   NEWTWEET It might be unrealistic but my weekend goals    NEWTWEET This is incredible    So grateful for working w  such amazing ppl  amp  for your amazing reviews      NEWTWEET Last time we were all in London together we were filming Justice League SENTENCEEND SENTENCESTART Can t believe the time is here  NEWTWEET What is she is doing with my tiara   NEWTWEET Press Tour Stop starts today in London       NEWTWEET It was such an honor to be able to stand with these real life Wonder Woman SENTENCEEND SENTENCESTART Thank you for letting me share this moment with you all  NEWTWEET Thank you for this amazing opportunity  NEWTWEET    NEWTWEET   love your work  NEWTWEET The Tour is off to an amazing start  Thank you China for having us         NEWTWEET China NEWTWEET      NEWTWEET Very much noted  She s a real life hero  Im sending her a huge hug of love  good energy and strength SENTENCEEND SENTENCESTART NEWTWEET RT An amazing event for a very personal cause SENTENCEEND SENTENCESTART My wonderful daughter Willow courageously leads our family as we walk to raise  NEWTWEET I m such a big fan SENTENCEEND SENTENCESTART You were so sweet today thanks for the awesome words  NEWTWEET NEWTWEET NEWTWEET It s finally here  Tonight I m hosting with musical guest Sam Smith   Make sure you tune in     Photos by Mary Ellen Matthews NEWTWEET RT Fun show tonight  is here  stop by  performs  amp  more  NEWTWEET Beyond excited to be hosting this Saturday with musical guest SENTENCEEND SENTENCESTART Make sure you tune in     NEWTWEET UK friends  You can get your hands on on Blu ray on October    Patty wasn t exaggerating when she said it was freezing cold SENTENCEEND SENTENCESTART    NEWTWEET RT NEW with  amp  more SENTENCEEND SENTENCESTART NEWTWEET NEWTWEET No longer a secret  so excited to be hosting NEWTWEET Shana Tova to all of you  May this year be filled with joy   happiness  creation and love   NEWTWEET Today is the day  is now available on Blu ray  I can't thank everyone enough for the love and support for this movie    NEWTWEET What an amazing ride this has been  See the rest of the outtakes on the DVD extras      SENTENCEEND SENTENCESTART NEWTWEET     drones lit up the LA sky tonight to celebrate Power  Grace  Wisdom  and Wonder SENTENCEEND SENTENCESTART Bring home on Digital Now and Blu ray       NEWTWEET So well deserved my sister SENTENCEEND SENTENCESTART Couldn't be happier for you SENTENCEEND SENTENCESTART Paving the right way for so many to come NEWTWEET Thank you Rachel for the kind words about Wonder Woman  I'm a big fan of yours  Xo NEWTWEET There truly is a little Wonder in ALL of us  NEWTWEET Chris's nickname for me was giggle Gadot   amp  I could communicate with no words SENTENCEEND SENTENCESTART I enjoyed filming every second of the movie SENTENCEEND SENTENCESTART       NEWTWEET Morgan and Meg are the perfect example of real life superheroes SENTENCEEND SENTENCESTART Sending my strength and positive energy to both of you  NEWTWEET Just a taste of the fun we had while filming  more to come with the DVD's extras  on       We weren't always so serious on Themyscira SENTENCEEND SENTENCESTART       NEWTWEET Looking amazing ladies         NEWTWEET RT The Warrior Princess has arrived SENTENCEEND SENTENCESTART Own on Digital Today  NEWTWEET It was an honor to present the Video of the Year award tonight at the   Congrats to and all the other nominees SENTENCEEND SENTENCESTART NEWTWEET RT NEWTWEET Wonder Woman was released in Japan  I hope you enjoy     NEWTWEET Such an honor SENTENCEEND SENTENCESTART Thank you   NEWTWEET So true SENTENCEEND SENTENCESTART NEWTWEET Wow  Just heard the news  Thank u to everyone who has shown their support to WW in theaters  What an amazing ride this has been  NEWTWEET Chasing Waterfalls    NEWTWEET Sending all my love to Barcelona and those affected by this horrible tragedy NEWTWEET Thank you for the support of Wonder Woman last night    awards  wow  We are blessed with fans like you  NEWTWEET Exciting news  is coming to Digital      and Blu ray       SENTENCEEND SENTENCESTART NEWTWEET Another day at work SENTENCEEND SENTENCESTART is raving  amp  is working  amp  I'm just trying to look cool SENTENCEEND SENTENCESTART Art Direction  NEWTWEET I hope you enjoyed watching the NEWTWEET to standing on the blue carpet with our fearless leader  Patty Jenkins  for the premiere SENTENCEEND SENTENCESTART NEWTWEET Amazing article   thank you    NEWTWEET You can't save the world alone     from NEWTWEET from way for justice SENTENCEEND SENTENCESTART Enjoy this sneak peak for now    In theaters November    SENTENCEEND SENTENCESTART NEWTWEET Ready or not here we come SENTENCEEND SENTENCESTART      benaffleck NEWTWEET These women are so inspirational  Love seeing them making an impact in the world of game design SENTENCEEND SENTENCESTART NEWTWEET to last year at    Can t wait to see you all this weekend   NEWTWEET I love stumbling across photos from    NEWTWEET It's the little things that make a difference   NEWTWEET Well said     NEWTWEET This is incredible    Thanks to all of you for making this possible    NEWTWEET    NEWTWEET Black and white glamour    NEWTWEET unite     from   NEWTWEET When it's finally Friday SENTENCEEND SENTENCESTART     NEWTWEET Thanks to ALL of you for making a success SENTENCEEND SENTENCESTART Wonder Woman has the best fans in the world    thx NEWTWEET Chocolate is always the answer    NEWTWEET NEWTWEET you're the best  Thank you for this SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART love you and so much         NEWTWEET Wow   A huge thank u to all for making a success    thx for this piece NEWTWEET Getting thrown into Monday like SENTENCEEND SENTENCESTART   NEWTWEET    NEWTWEET We had fun    Here is some more exclusive from     Enjoy   NEWTWEET Kisses from my Lola    NEWTWEET NEWTWEET    NEWTWEET The smiling skies   NEWTWEET Hope everyone is having a wonderful shared with family and friends      NEWTWEET Hello  July     NEWTWEET Weekend got me like SENTENCEEND SENTENCESTART    NEWTWEET Always laughing with these two    NEWTWEET NEWTWEET Another from      NEWTWEET Relaxing and enjoying some scenery     NEWTWEET      NEWTWEET So excited I finally get to share with you  Spain      disfrutar    NEWTWEET from   NEWTWEET to this day    NEWTWEET Peek a boo    NEWTWEET Happiness      NEWTWEET   NEWTWEET Me   you   forever    NEWTWEET from Here s some BTS footage from NEWTWEET To my other half   father of my children and love of my life SENTENCEEND SENTENCESTART You are simply the  NEWTWEET Trying to hypnotize you SENTENCEEND SENTENCESTART NEWTWEET Sleepless night   colic   months old baby and an early wake up by my   year old SENTENCEEND SENTENCESTART Went to the  NEWTWEET Wow  Thx so much NEWTWEET It was always such a joy working with my family      We had a blast     NEWTWEET And one more photo from   NEWTWEET Wow  This is incredible    Bravo NEWTWEET Make sure to always eat your greens   NEWTWEET This is awesome    NEWTWEET Wow the last paragraph really gave me the chills SENTENCEEND SENTENCESTART So true SENTENCEEND SENTENCESTART So powerful SENTENCEEND SENTENCESTART Gives me a huge drive to dive in and work on the next one SENTENCEEND SENTENCESTART SENTENCEEND SENTENCESTART    NEWTWEET Pure joy working on bc I was surrounded by great people   NEWTWEET Thank you for having me Grazia China    NEWTWEET RT It's official  has become the most Tweeted movie of      SENTENCEEND SENTENCESTART Congrats to and the cas  NEWTWEET Can t thank you all enough for making     NEWTWEET     thx for this cool piece   NEWTWEET To my fans SENTENCEEND SENTENCESTART Thank you all  I love you all so much                     Merci  Danke      Gracias        Obrigado    NEWTWEET What was your favorite part of costume     from NEWTWEET from  I thought she knew me     NEWTWEET Thanks so much my friend     NEWTWEET Take that     NEWTWEET        NEWTWEET Wow SENTENCEEND SENTENCESTART Thank you so much SENTENCEEND SENTENCESTART Means a lot coming from you    NEWTWEET      NEWTWEET Thank you so much James  so happy you enjoyed it SENTENCEEND SENTENCESTART Can't wait for Aquaman    NEWTWEET So happy you liked it Josh   NEWTWEET Wow thank you so much    sending you a kiss back   NEWTWEET WOW  This is so moving and inspiring  NEWTWEET LOL  thank you so much  c u soon   NEWTWEET Looking good girl  NEWTWEET It looks so good when you do it      NEWTWEET The cutest   NEWTWEET I always knew you were a smart guy    But I think its worth a fight SENTENCEEND SENTENCESTART we should collide worlds  NEWTWEET Wow thank you sister SENTENCEEND SENTENCESTART I'm a big fan of your work and I'm so happy you enjoyed it   NEWTWEET SENTENCEEND SENTENCESTART Thank you so much for the love   Miss you big guy     NEWTWEET Can't believe how much fun and a joy it was making this movie SENTENCEEND SENTENCESTART Truly blessed to work with these amazing people SENTENCEEND SENTENCESTART NEWTWEET Hello players and fans  I am so happy to be the spokesperson for League of Angels   Paradise Land available now for all of you to play  NEWTWEET TODAY is the day      is officially in theaters everywhere SENTENCEEND SENTENCESTART Much love to all     NEWTWEET Its almost time     So thrilled to share with you all the latest poster for    NEWTWEET Add a bit more Wonder to your text messages with the new stickers    Stickers available for iOS and Andriod download now    NEWTWEET Now this is a super squad     Thx    in theaters tomorrow  NEWTWEET Thx for this feature    Im honored to work alongside someone who I admire SENTENCEEND SENTENCESTART Love u     NEWTWEET Loved having you by my side      Muah   NEWTWEET I'm so excited to share my cover with you all SENTENCEEND SENTENCESTART Thx so much       Photo taken by"



text_to_word_sequence splits a sentence into a list of words, for every of our famous sequence.
We do this in order to use the tokenizer of keras to create new tweets for every famous.


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




    ['NEWTWEET', 'I', 'grew', 'up', 'watching', 'and', 'now', 'I', 'get', 'to']



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

lets create the vocabulary, we create a vocabulary for each famous.


```python
import pandas as pd
import numpy as np
trump_vocab = pd.DataFrame({'word':trump_text_splitted,'code':np.argmax(trump_text_mtx,axis=1)})

hilary_vocab = pd.DataFrame({'word':hilary_text_splitted,'code':np.argmax(hilary_text_mtx,axis=1)})

obama_vocab = pd.DataFrame({'word':obama_text_splitted,'code':np.argmax(obama_text_mtx,axis=1)})

seinfeld_vocab = pd.DataFrame({'word':seinfeld_text_splitted,'code':np.argmax(sienfeld_text_mtx,axis=1)})

gadot_vocab = pd.DataFrame({'word':gadot_text_splitted,'code':np.argmax(gadot_text_mtx,axis=1)})
```

Now we need to import the keras libraries we will use in order to create and use our future models to create the tweets for each famous.


```python
from keras.models import Sequential
from keras.layers.core import Dense, Activation, Flatten
from keras.layers.wrappers import TimeDistributed
from keras.layers.embeddings import Embedding
from keras.layers.recurrent import LSTM
from keras.layers.embeddings import Embedding
from keras.layers.recurrent import SimpleRNN
```

We will create a sequential model for each famous, in order to create for each one of them his new tweets.


```python
trump_model = Sequential()

hilary_model = Sequential()

obama_model = Sequential()

seinfeld_model = Sequential()

gadot_model = Sequential()
```

We add an embedding layer to each model, we set the input by each famous input matrix, and the dimension of the input to be the number of rows in the matrix.


```python
trump_model.add(Embedding(input_dim=trump_input.shape[1],output_dim= 42, input_length=trump_input.shape[1]))

hilary_model.add(Embedding(input_dim=hilary_input.shape[1],output_dim= 42, input_length=hilary_input.shape[1]))

obama_model.add(Embedding(input_dim=obama_input.shape[1],output_dim= 42, input_length=obama_input.shape[1]))

seinfeld_model.add(Embedding(input_dim=seinfeld_input.shape[1],output_dim= 42, input_length=seinfeld_input.shape[1]))

gadot_model.add(Embedding(input_dim=gadot_input.shape[1],output_dim= 42, input_length=gadot_input.shape[1]))
```

We add an flatten and dense layer to each model, we set the dense of each one by each famous output matrix number of rows.


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

Now we will plot a summary for each model, in order to learn things about each famous model.


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
    

We will compile each model with the rmsprop parameter in order to generate the next word in the tweet, by the word with the best probability. 


```python
trump_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

hilary_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

obama_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

seinfeld_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])

gadot_model.compile(loss='categorical_crossentropy', optimizer='rmsprop',metrics=["accuracy"])
```

Now we will fit every model by the input and output matrix. We set the number of iteration to 50 in each model in order to avoid overfitting , and avoid long run times.


```python
trump_model.fit(trump_input, y=trump_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

hilary_model.fit(hilary_input, y=hilary_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

obama_model.fit(obama_input, y=obama_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

seinfeld_model.fit(seinfeld_input, y=seinfeld_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)

gadot_model.fit(gadot_input, y=gadot_output, batch_size=200, nb_epoch=50, verbose=1, validation_split=0.2)
```

    Train on 5651 samples, validate on 1413 samples
    Epoch 1/50
    5651/5651 [==============================] - 2s - loss: 2.9851 - acc: 0.0487 - val_loss: 2.8057 - val_acc: 0.1005
    Epoch 2/50
    5651/5651 [==============================] - 2s - loss: 2.8729 - acc: 0.0669 - val_loss: 2.7989 - val_acc: 0.0502
    Epoch 3/50
    5651/5651 [==============================] - 1s - loss: 2.8522 - acc: 0.0494 - val_loss: 2.7749 - val_acc: 0.0502
    Epoch 4/50
    5651/5651 [==============================] - 2s - loss: 2.8263 - acc: 0.0494 - val_loss: 2.7540 - val_acc: 0.0502
    Epoch 5/50
    5651/5651 [==============================] - 2s - loss: 2.7889 - acc: 0.0494 - val_loss: 2.7304 - val_acc: 0.0502
    Epoch 6/50
    5651/5651 [==============================] - 1s - loss: 2.7297 - acc: 0.1088 - val_loss: 2.6768 - val_acc: 0.1005
    Epoch 7/50
    5651/5651 [==============================] - 1s - loss: 2.6555 - acc: 0.1134 - val_loss: 2.6044 - val_acc: 0.1210
    Epoch 8/50
    5651/5651 [==============================] - 1s - loss: 2.5753 - acc: 0.1255 - val_loss: 2.5478 - val_acc: 0.1309
    Epoch 9/50
    5651/5651 [==============================] - 1s - loss: 2.5021 - acc: 0.1347 - val_loss: 2.5043 - val_acc: 0.1260
    Epoch 10/50
    5651/5651 [==============================] - 2s - loss: 2.4361 - acc: 0.1435 - val_loss: 2.4624 - val_acc: 0.1380
    Epoch 11/50
    5651/5651 [==============================] - 1s - loss: 2.3683 - acc: 0.1515 - val_loss: 2.4550 - val_acc: 0.1189
    Epoch 12/50
    5651/5651 [==============================] - 1s - loss: 2.3087 - acc: 0.1601 - val_loss: 2.4238 - val_acc: 0.1394
    Epoch 13/50
    5651/5651 [==============================] - 2s - loss: 2.2463 - acc: 0.1603 - val_loss: 2.3641 - val_acc: 0.1515
    Epoch 14/50
    5651/5651 [==============================] - 2s - loss: 2.1946 - acc: 0.1688 - val_loss: 2.3568 - val_acc: 0.1472
    Epoch 15/50
    5651/5651 [==============================] - 1s - loss: 2.1489 - acc: 0.1750 - val_loss: 2.3440 - val_acc: 0.15070.17
    Epoch 16/50
    5651/5651 [==============================] - 1s - loss: 2.1045 - acc: 0.1763 - val_loss: 2.3253 - val_acc: 0.1486
    Epoch 17/50
    5651/5651 [==============================] - 1s - loss: 2.0662 - acc: 0.1805 - val_loss: 2.3015 - val_acc: 0.1522
    Epoch 18/50
    5651/5651 [==============================] - 1s - loss: 2.0310 - acc: 0.1856 - val_loss: 2.2974 - val_acc: 0.1592
    Epoch 19/50
    5651/5651 [==============================] - 1s - loss: 2.0024 - acc: 0.1876 - val_loss: 2.2994 - val_acc: 0.1451
    Epoch 20/50
    5651/5651 [==============================] - 1s - loss: 1.9726 - acc: 0.1878 - val_loss: 2.3023 - val_acc: 0.1479
    Epoch 21/50
    5651/5651 [==============================] - 2s - loss: 1.9501 - acc: 0.1890 - val_loss: 2.3078 - val_acc: 0.1408
    Epoch 22/50
    5651/5651 [==============================] - 2s - loss: 1.9279 - acc: 0.1902 - val_loss: 2.3271 - val_acc: 0.1515
    Epoch 23/50
    5651/5651 [==============================] - 1s - loss: 1.9083 - acc: 0.1885 - val_loss: 2.3133 - val_acc: 0.1529
    Epoch 24/50
    5651/5651 [==============================] - 1s - loss: 1.8916 - acc: 0.1908 - val_loss: 2.3061 - val_acc: 0.1401
    Epoch 25/50
    5651/5651 [==============================] - 2s - loss: 1.8762 - acc: 0.1897 - val_loss: 2.3190 - val_acc: 0.1550
    Epoch 26/50
    5651/5651 [==============================] - 2s - loss: 1.8628 - acc: 0.1906 - val_loss: 2.3328 - val_acc: 0.1592
    Epoch 27/50
    5651/5651 [==============================] - 2s - loss: 1.8522 - acc: 0.1922 - val_loss: 2.3440 - val_acc: 0.1444
    Epoch 28/50
    5651/5651 [==============================] - 2s - loss: 1.8400 - acc: 0.1936 - val_loss: 2.3366 - val_acc: 0.1507
    Epoch 29/50
    5651/5651 [==============================] - 2s - loss: 1.8295 - acc: 0.1931 - val_loss: 2.3514 - val_acc: 0.1543
    Epoch 30/50
    5651/5651 [==============================] - 2s - loss: 1.8205 - acc: 0.1915 - val_loss: 2.3601 - val_acc: 0.1522
    Epoch 31/50
    5651/5651 [==============================] - 2s - loss: 1.8150 - acc: 0.1909 - val_loss: 2.3768 - val_acc: 0.1536
    Epoch 32/50
    5651/5651 [==============================] - 1s - loss: 1.8043 - acc: 0.1938 - val_loss: 2.4030 - val_acc: 0.1557
    Epoch 33/50
    5651/5651 [==============================] - 2s - loss: 1.8035 - acc: 0.1938 - val_loss: 2.3906 - val_acc: 0.1550
    Epoch 34/50
    5651/5651 [==============================] - 2s - loss: 1.7942 - acc: 0.1934 - val_loss: 2.4095 - val_acc: 0.1522
    Epoch 35/50
    5651/5651 [==============================] - 2s - loss: 1.7900 - acc: 0.1927 - val_loss: 2.4030 - val_acc: 0.1458
    Epoch 36/50
    5651/5651 [==============================] - 2s - loss: 1.7866 - acc: 0.1911 - val_loss: 2.4208 - val_acc: 0.1500
    Epoch 37/50
    5651/5651 [==============================] - 2s - loss: 1.7808 - acc: 0.1920 - val_loss: 2.4240 - val_acc: 0.1543
    Epoch 38/50
    5651/5651 [==============================] - 2s - loss: 1.7789 - acc: 0.1920 - val_loss: 2.4283 - val_acc: 0.1522
    Epoch 39/50
    5651/5651 [==============================] - 2s - loss: 1.7761 - acc: 0.1929 - val_loss: 2.4369 - val_acc: 0.1557
    Epoch 40/50
    5651/5651 [==============================] - 2s - loss: 1.7731 - acc: 0.1927 - val_loss: 2.4755 - val_acc: 0.1500
    Epoch 41/50
    5651/5651 [==============================] - 2s - loss: 1.7699 - acc: 0.1925 - val_loss: 2.4575 - val_acc: 0.1529
    Epoch 42/50
    5651/5651 [==============================] - 2s - loss: 1.7681 - acc: 0.1915 - val_loss: 2.4473 - val_acc: 0.1529
    Epoch 43/50
    5651/5651 [==============================] - 2s - loss: 1.7652 - acc: 0.1916 - val_loss: 2.4682 - val_acc: 0.1536
    Epoch 44/50
    5651/5651 [==============================] - 2s - loss: 1.7643 - acc: 0.1936 - val_loss: 2.5034 - val_acc: 0.1571
    Epoch 45/50
    5651/5651 [==============================] - 2s - loss: 1.7630 - acc: 0.1924 - val_loss: 2.4836 - val_acc: 0.1507
    Epoch 46/50
    5651/5651 [==============================] - 2s - loss: 1.7613 - acc: 0.1902 - val_loss: 2.5082 - val_acc: 0.1607 ETA: 0s - loss: 1.7499 - acc: 0.
    Epoch 47/50
    5651/5651 [==============================] - 2s - loss: 1.7592 - acc: 0.1932 - val_loss: 2.4712 - val_acc: 0.1543
    Epoch 48/50
    5651/5651 [==============================] - 2s - loss: 1.7588 - acc: 0.1943 - val_loss: 2.4868 - val_acc: 0.1550
    Epoch 49/50
    5651/5651 [==============================] - 2s - loss: 1.7572 - acc: 0.1929 - val_loss: 2.4877 - val_acc: 0.1529
    Epoch 50/50
    5651/5651 [==============================] - 2s - loss: 1.7564 - acc: 0.1920 - val_loss: 2.5222 - val_acc: 0.1578
    Train on 3858 samples, validate on 965 samples
    Epoch 1/50
    3858/3858 [==============================] - 1s - loss: 3.0454 - acc: 0.0303 - val_loss: 2.8835 - val_acc: 0.0321
    Epoch 2/50
    3858/3858 [==============================] - 1s - loss: 2.9132 - acc: 0.0438 - val_loss: 2.8413 - val_acc: 0.1306
    Epoch 3/50
    3858/3858 [==============================] - 1s - loss: 2.8907 - acc: 0.0679 - val_loss: 2.8327 - val_acc: 0.1306
    Epoch 4/50
    3858/3858 [==============================] - 1s - loss: 2.8756 - acc: 0.0575 - val_loss: 2.8269 - val_acc: 0.0653
    Epoch 5/50
    3858/3858 [==============================] - 1s - loss: 2.8605 - acc: 0.0508 - val_loss: 2.8036 - val_acc: 0.0653
    Epoch 6/50
    3858/3858 [==============================] - 1s - loss: 2.8387 - acc: 0.0508 - val_loss: 2.8040 - val_acc: 0.0653
    Epoch 7/50
    3858/3858 [==============================] - 1s - loss: 2.8072 - acc: 0.0747 - val_loss: 2.7572 - val_acc: 0.1617
    Epoch 8/50
    3858/3858 [==============================] - 1s - loss: 2.7587 - acc: 0.1257 - val_loss: 2.7257 - val_acc: 0.1420
    Epoch 9/50
    3858/3858 [==============================] - 1s - loss: 2.7077 - acc: 0.1280 - val_loss: 2.6415 - val_acc: 0.1617
    Epoch 10/50
    3858/3858 [==============================] - 1s - loss: 2.6479 - acc: 0.1350 - val_loss: 2.5911 - val_acc: 0.1751
    Epoch 11/50
    3858/3858 [==============================] - 1s - loss: 2.5983 - acc: 0.1384 - val_loss: 2.5714 - val_acc: 0.1710
    Epoch 12/50
    3858/3858 [==============================] - 1s - loss: 2.5439 - acc: 0.1418 - val_loss: 2.5114 - val_acc: 0.1492
    Epoch 13/50
    3858/3858 [==============================] - 1s - loss: 2.4987 - acc: 0.1467 - val_loss: 2.4902 - val_acc: 0.1907
    Epoch 14/50
    3858/3858 [==============================] - 1s - loss: 2.4571 - acc: 0.1540 - val_loss: 2.4967 - val_acc: 0.1461
    Epoch 15/50
    3858/3858 [==============================] - 1s - loss: 2.4119 - acc: 0.1534 - val_loss: 2.4442 - val_acc: 0.1896
    Epoch 16/50
    3858/3858 [==============================] - 1s - loss: 2.3727 - acc: 0.1625 - val_loss: 2.4503 - val_acc: 0.1679
    Epoch 17/50
    3858/3858 [==============================] - 1s - loss: 2.3324 - acc: 0.1656 - val_loss: 2.4313 - val_acc: 0.1523
    Epoch 18/50
    3858/3858 [==============================] - 1s - loss: 2.3031 - acc: 0.1664 - val_loss: 2.3932 - val_acc: 0.1938
    Epoch 19/50
    3858/3858 [==============================] - 1s - loss: 2.2632 - acc: 0.1768 - val_loss: 2.3670 - val_acc: 0.1969
    Epoch 20/50
    3858/3858 [==============================] - 1s - loss: 2.2333 - acc: 0.1814 - val_loss: 2.3882 - val_acc: 0.1803
    Epoch 21/50
    3858/3858 [==============================] - 1s - loss: 2.2010 - acc: 0.1838 - val_loss: 2.3638 - val_acc: 0.1917
    Epoch 22/50
    3858/3858 [==============================] - 1s - loss: 2.1678 - acc: 0.1921 - val_loss: 2.3812 - val_acc: 0.1948
    Epoch 23/50
    3858/3858 [==============================] - 1s - loss: 2.1405 - acc: 0.1897 - val_loss: 2.3146 - val_acc: 0.19790.
    Epoch 24/50
    3858/3858 [==============================] - 1s - loss: 2.1133 - acc: 0.1910 - val_loss: 2.3292 - val_acc: 0.2021
    Epoch 25/50
    3858/3858 [==============================] - 1s - loss: 2.0857 - acc: 0.1918 - val_loss: 2.3333 - val_acc: 0.1990
    Epoch 26/50
    3858/3858 [==============================] - 1s - loss: 2.0620 - acc: 0.1970 - val_loss: 2.3223 - val_acc: 0.1979
    Epoch 27/50
    3858/3858 [==============================] - 1s - loss: 2.0414 - acc: 0.1939 - val_loss: 2.3014 - val_acc: 0.1907
    Epoch 28/50
    3858/3858 [==============================] - 1s - loss: 2.0220 - acc: 0.1991 - val_loss: 2.3098 - val_acc: 0.2010
    Epoch 29/50
    3858/3858 [==============================] - 1s - loss: 1.9989 - acc: 0.2011 - val_loss: 2.3254 - val_acc: 0.1969
    Epoch 30/50
    3858/3858 [==============================] - 1s - loss: 1.9804 - acc: 0.2030 - val_loss: 2.3001 - val_acc: 0.2083
    Epoch 31/50
    3858/3858 [==============================] - 1s - loss: 1.9616 - acc: 0.2019 - val_loss: 2.3209 - val_acc: 0.2041
    Epoch 32/50
    3858/3858 [==============================] - 1s - loss: 1.9452 - acc: 0.2030 - val_loss: 2.3223 - val_acc: 0.2021
    Epoch 33/50
    3858/3858 [==============================] - 1s - loss: 1.9292 - acc: 0.2024 - val_loss: 2.3190 - val_acc: 0.1959
    Epoch 34/50
    3858/3858 [==============================] - 1s - loss: 1.9117 - acc: 0.2058 - val_loss: 2.3233 - val_acc: 0.1927
    Epoch 35/50
    3858/3858 [==============================] - 1s - loss: 1.8979 - acc: 0.2043 - val_loss: 2.3295 - val_acc: 0.1948
    Epoch 36/50
    3858/3858 [==============================] - 1s - loss: 1.8847 - acc: 0.2024 - val_loss: 2.3514 - val_acc: 0.1731
    Epoch 37/50
    3858/3858 [==============================] - 1s - loss: 1.8716 - acc: 0.2063 - val_loss: 2.3357 - val_acc: 0.1948
    Epoch 38/50
    3858/3858 [==============================] - 1s - loss: 1.8625 - acc: 0.2017 - val_loss: 2.3349 - val_acc: 0.2021
    Epoch 39/50
    3858/3858 [==============================] - 1s - loss: 1.8511 - acc: 0.2014 - val_loss: 2.3549 - val_acc: 0.1948
    Epoch 40/50
    3858/3858 [==============================] - 1s - loss: 1.8407 - acc: 0.2058 - val_loss: 2.3646 - val_acc: 0.1979
    Epoch 41/50
    3858/3858 [==============================] - 1s - loss: 1.8327 - acc: 0.2006 - val_loss: 2.3866 - val_acc: 0.2010
    Epoch 42/50
    3858/3858 [==============================] - 1s - loss: 1.8223 - acc: 0.2032 - val_loss: 2.3973 - val_acc: 0.2010
    Epoch 43/50
    3858/3858 [==============================] - 1s - loss: 1.8136 - acc: 0.2043 - val_loss: 2.4062 - val_acc: 0.2010
    Epoch 44/50
    3858/3858 [==============================] - 1s - loss: 1.8101 - acc: 0.2050 - val_loss: 2.4284 - val_acc: 0.1959
    Epoch 45/50
    3858/3858 [==============================] - 1s - loss: 1.8056 - acc: 0.2006 - val_loss: 2.4316 - val_acc: 0.1979
    Epoch 46/50
    3858/3858 [==============================] - 1s - loss: 1.7988 - acc: 0.2017 - val_loss: 2.4352 - val_acc: 0.2052
    Epoch 47/50
    3858/3858 [==============================] - 1s - loss: 1.7918 - acc: 0.2037 - val_loss: 2.4408 - val_acc: 0.1938
    Epoch 48/50
    3858/3858 [==============================] - 1s - loss: 1.7900 - acc: 0.2035 - val_loss: 2.4572 - val_acc: 0.1917
    Epoch 49/50
    3858/3858 [==============================] - 1s - loss: 1.7862 - acc: 0.2009 - val_loss: 2.4603 - val_acc: 0.2000
    Epoch 50/50
    3858/3858 [==============================] - 1s - loss: 1.7804 - acc: 0.2009 - val_loss: 2.4763 - val_acc: 0.2031
    Train on 3361 samples, validate on 841 samples
    Epoch 1/50
    3361/3361 [==============================] - 1s - loss: 3.0266 - acc: 0.0500 - val_loss: 3.0564 - val_acc: 0.1034
    Epoch 2/50
    3361/3361 [==============================] - 1s - loss: 2.8475 - acc: 0.0777 - val_loss: 3.0108 - val_acc: 0.0559
    Epoch 3/50
    3361/3361 [==============================] - 1s - loss: 2.8274 - acc: 0.0818 - val_loss: 3.0058 - val_acc: 0.0559
    Epoch 4/50
    3361/3361 [==============================] - 1s - loss: 2.8150 - acc: 0.0598 - val_loss: 2.9852 - val_acc: 0.0559
    Epoch 5/50
    3361/3361 [==============================] - 1s - loss: 2.7982 - acc: 0.0598 - val_loss: 2.9976 - val_acc: 0.0559
    Epoch 6/50
    3361/3361 [==============================] - 1s - loss: 2.7829 - acc: 0.0598 - val_loss: 2.9457 - val_acc: 0.0559
    Epoch 7/50
    3361/3361 [==============================] - 1s - loss: 2.7573 - acc: 0.0598 - val_loss: 2.9439 - val_acc: 0.0559
    Epoch 8/50
    3361/3361 [==============================] - 1s - loss: 2.7226 - acc: 0.1145 - val_loss: 2.9177 - val_acc: 0.1403
    Epoch 9/50
    3361/3361 [==============================] - 1s - loss: 2.6721 - acc: 0.1446 - val_loss: 2.8581 - val_acc: 0.1106
    Epoch 10/50
    3361/3361 [==============================] - 1s - loss: 2.6203 - acc: 0.1488 - val_loss: 2.8253 - val_acc: 0.1677
    Epoch 11/50
    3361/3361 [==============================] - 1s - loss: 2.5650 - acc: 0.1568 - val_loss: 2.8070 - val_acc: 0.1558
    Epoch 12/50
    3361/3361 [==============================] - 1s - loss: 2.5153 - acc: 0.1556 - val_loss: 2.7269 - val_acc: 0.1617
    Epoch 13/50
    3361/3361 [==============================] - 1s - loss: 2.4658 - acc: 0.1636 - val_loss: 2.6995 - val_acc: 0.1486
    Epoch 14/50
    3361/3361 [==============================] - 1s - loss: 2.4148 - acc: 0.1648 - val_loss: 2.6678 - val_acc: 0.1688
    Epoch 15/50
    3361/3361 [==============================] - 1s - loss: 2.3714 - acc: 0.1711 - val_loss: 2.6496 - val_acc: 0.1653
    Epoch 16/50
    3361/3361 [==============================] - 1s - loss: 2.3304 - acc: 0.1764 - val_loss: 2.6076 - val_acc: 0.1641
    Epoch 17/50
    3361/3361 [==============================] - 1s - loss: 2.2829 - acc: 0.1735 - val_loss: 2.5808 - val_acc: 0.1451
    Epoch 18/50
    3361/3361 [==============================] - 1s - loss: 2.2489 - acc: 0.1833 - val_loss: 2.5563 - val_acc: 0.1772
    Epoch 19/50
    3361/3361 [==============================] - 1s - loss: 2.2032 - acc: 0.1904 - val_loss: 2.5305 - val_acc: 0.1819
    Epoch 20/50
    3361/3361 [==============================] - 1s - loss: 2.1652 - acc: 0.1946 - val_loss: 2.4958 - val_acc: 0.1938
    Epoch 21/50
    3361/3361 [==============================] - 1s - loss: 2.1305 - acc: 0.2005 - val_loss: 2.4912 - val_acc: 0.1926
    Epoch 22/50
    3361/3361 [==============================] - 1s - loss: 2.0974 - acc: 0.2092 - val_loss: 2.4981 - val_acc: 0.2010
    Epoch 23/50
    3361/3361 [==============================] - 1s - loss: 2.0657 - acc: 0.2077 - val_loss: 2.4672 - val_acc: 0.1974
    Epoch 24/50
    3361/3361 [==============================] - 1s - loss: 2.0339 - acc: 0.2130 - val_loss: 2.4350 - val_acc: 0.1938
    Epoch 25/50
    3361/3361 [==============================] - 1s - loss: 2.0070 - acc: 0.2178 - val_loss: 2.4306 - val_acc: 0.2010
    Epoch 26/50
    3361/3361 [==============================] - 1s - loss: 1.9766 - acc: 0.2214 - val_loss: 2.4159 - val_acc: 0.2105
    Epoch 27/50
    3361/3361 [==============================] - 1s - loss: 1.9553 - acc: 0.2220 - val_loss: 2.4303 - val_acc: 0.2057
    Epoch 28/50
    3361/3361 [==============================] - 1s - loss: 1.9319 - acc: 0.2234 - val_loss: 2.4055 - val_acc: 0.2128
    Epoch 29/50
    3361/3361 [==============================] - 1s - loss: 1.9098 - acc: 0.2288 - val_loss: 2.3823 - val_acc: 0.2081
    Epoch 30/50
    3361/3361 [==============================] - 1s - loss: 1.8887 - acc: 0.2273 - val_loss: 2.3986 - val_acc: 0.2069
    Epoch 31/50
    3361/3361 [==============================] - 1s - loss: 1.8709 - acc: 0.2300 - val_loss: 2.3955 - val_acc: 0.2069
    Epoch 32/50
    3361/3361 [==============================] - 1s - loss: 1.8499 - acc: 0.2348 - val_loss: 2.3967 - val_acc: 0.2093
    Epoch 33/50
    3361/3361 [==============================] - 1s - loss: 1.8383 - acc: 0.2324 - val_loss: 2.4023 - val_acc: 0.2069
    Epoch 34/50
    3361/3361 [==============================] - 1s - loss: 1.8222 - acc: 0.2356 - val_loss: 2.3842 - val_acc: 0.2128
    Epoch 35/50
    3361/3361 [==============================] - 1s - loss: 1.8077 - acc: 0.2339 - val_loss: 2.3895 - val_acc: 0.2105
    Epoch 36/50
    3361/3361 [==============================] - 1s - loss: 1.7947 - acc: 0.2377 - val_loss: 2.3881 - val_acc: 0.2128
    Epoch 37/50
    3361/3361 [==============================] - 1s - loss: 1.7840 - acc: 0.2353 - val_loss: 2.4068 - val_acc: 0.2152
    Epoch 38/50
    3361/3361 [==============================] - 1s - loss: 1.7732 - acc: 0.2392 - val_loss: 2.4122 - val_acc: 0.2081
    Epoch 39/50
    3361/3361 [==============================] - 1s - loss: 1.7615 - acc: 0.2374 - val_loss: 2.4221 - val_acc: 0.2117
    Epoch 40/50
    3361/3361 [==============================] - 1s - loss: 1.7505 - acc: 0.2386 - val_loss: 2.4001 - val_acc: 0.2128
    Epoch 41/50
    3361/3361 [==============================] - 1s - loss: 1.7435 - acc: 0.2395 - val_loss: 2.4164 - val_acc: 0.2152
    Epoch 42/50
    3361/3361 [==============================] - 1s - loss: 1.7338 - acc: 0.2350 - val_loss: 2.4221 - val_acc: 0.2128
    Epoch 43/50
    3361/3361 [==============================] - 1s - loss: 1.7251 - acc: 0.2377 - val_loss: 2.4351 - val_acc: 0.2140
    Epoch 44/50
    3361/3361 [==============================] - 1s - loss: 1.7196 - acc: 0.2374 - val_loss: 2.4521 - val_acc: 0.2117
    Epoch 45/50
    3361/3361 [==============================] - 1s - loss: 1.7129 - acc: 0.2345 - val_loss: 2.4556 - val_acc: 0.2105
    Epoch 46/50
    3361/3361 [==============================] - 1s - loss: 1.7060 - acc: 0.2356 - val_loss: 2.4539 - val_acc: 0.2117
    Epoch 47/50
    3361/3361 [==============================] - 1s - loss: 1.7011 - acc: 0.2321 - val_loss: 2.4460 - val_acc: 0.2176
    Epoch 48/50
    3361/3361 [==============================] - 1s - loss: 1.6962 - acc: 0.2404 - val_loss: 2.4647 - val_acc: 0.2117
    Epoch 49/50
    3361/3361 [==============================] - 1s - loss: 1.6888 - acc: 0.2368 - val_loss: 2.4730 - val_acc: 0.2188
    Epoch 50/50
    3361/3361 [==============================] - 1s - loss: 1.6889 - acc: 0.2386 - val_loss: 2.4706 - val_acc: 0.2105
    Train on 3043 samples, validate on 761 samples
    Epoch 1/50
    3043/3043 [==============================] - 1s - loss: 3.2174 - acc: 0.0697 - val_loss: 2.8542 - val_acc: 0.0828
    Epoch 2/50
    3043/3043 [==============================] - 1s - loss: 2.9793 - acc: 0.0940 - val_loss: 2.7739 - val_acc: 0.0828
    Epoch 3/50
    3043/3043 [==============================] - 1s - loss: 2.9255 - acc: 0.0940 - val_loss: 2.7910 - val_acc: 0.0828
    Epoch 4/50
    3043/3043 [==============================] - 1s - loss: 2.9033 - acc: 0.0940 - val_loss: 2.7621 - val_acc: 0.0828
    Epoch 5/50
    3043/3043 [==============================] - 1s - loss: 2.8820 - acc: 0.0940 - val_loss: 2.7302 - val_acc: 0.0828
    Epoch 6/50
    3043/3043 [==============================] - 1s - loss: 2.8498 - acc: 0.0940 - val_loss: 2.7033 - val_acc: 0.0828
    Epoch 7/50
    3043/3043 [==============================] - 1s - loss: 2.8066 - acc: 0.1518 - val_loss: 2.6944 - val_acc: 0.1984
    Epoch 8/50
    3043/3043 [==============================] - 1s - loss: 2.7483 - acc: 0.2087 - val_loss: 2.5910 - val_acc: 0.1984
    Epoch 9/50
    3043/3043 [==============================] - 1s - loss: 2.6775 - acc: 0.2192 - val_loss: 2.5661 - val_acc: 0.1984
    Epoch 10/50
    3043/3043 [==============================] - 1s - loss: 2.6129 - acc: 0.2264 - val_loss: 2.5068 - val_acc: 0.2339
    Epoch 11/50
    3043/3043 [==============================] - 1s - loss: 2.5473 - acc: 0.2386 - val_loss: 2.4773 - val_acc: 0.2300
    Epoch 12/50
    3043/3043 [==============================] - 1s - loss: 2.4882 - acc: 0.2458 - val_loss: 2.4192 - val_acc: 0.2116
    Epoch 13/50
    3043/3043 [==============================] - 1s - loss: 2.4339 - acc: 0.2507 - val_loss: 2.4069 - val_acc: 0.2260
    Epoch 14/50
    3043/3043 [==============================] - 1s - loss: 2.3863 - acc: 0.2560 - val_loss: 2.3373 - val_acc: 0.2589
    Epoch 15/50
    3043/3043 [==============================] - 1s - loss: 2.3356 - acc: 0.2596 - val_loss: 2.3058 - val_acc: 0.2602
    Epoch 16/50
    3043/3043 [==============================] - 1s - loss: 2.2858 - acc: 0.2603 - val_loss: 2.3050 - val_acc: 0.2576
    Epoch 17/50
    3043/3043 [==============================] - 1s - loss: 2.2449 - acc: 0.2622 - val_loss: 2.2692 - val_acc: 0.2549
    Epoch 18/50
    3043/3043 [==============================] - 1s - loss: 2.1961 - acc: 0.2619 - val_loss: 2.2727 - val_acc: 0.2549
    Epoch 19/50
    3043/3043 [==============================] - 1s - loss: 2.1675 - acc: 0.2636 - val_loss: 2.2215 - val_acc: 0.2510
    Epoch 20/50
    3043/3043 [==============================] - 1s - loss: 2.1224 - acc: 0.2659 - val_loss: 2.1942 - val_acc: 0.2615
    Epoch 21/50
    3043/3043 [==============================] - 1s - loss: 2.0893 - acc: 0.2695 - val_loss: 2.1831 - val_acc: 0.2668
    Epoch 22/50
    3043/3043 [==============================] - 1s - loss: 2.0540 - acc: 0.2757 - val_loss: 2.1911 - val_acc: 0.2562
    Epoch 23/50
    3043/3043 [==============================] - 1s - loss: 2.0222 - acc: 0.2754 - val_loss: 2.1906 - val_acc: 0.2668
    Epoch 24/50
    3043/3043 [==============================] - 1s - loss: 1.9970 - acc: 0.2823 - val_loss: 2.1445 - val_acc: 0.2681
    Epoch 25/50
    3043/3043 [==============================] - 1s - loss: 1.9671 - acc: 0.2826 - val_loss: 2.1667 - val_acc: 0.2628
    Epoch 26/50
    3043/3043 [==============================] - 1s - loss: 1.9410 - acc: 0.2905 - val_loss: 2.1297 - val_acc: 0.2589
    Epoch 27/50
    3043/3043 [==============================] - 1s - loss: 1.9116 - acc: 0.2879 - val_loss: 2.1467 - val_acc: 0.2681
    Epoch 28/50
    3043/3043 [==============================] - 1s - loss: 1.8961 - acc: 0.2938 - val_loss: 2.1661 - val_acc: 0.2602
    Epoch 29/50
    3043/3043 [==============================] - 1s - loss: 1.8688 - acc: 0.2964 - val_loss: 2.1537 - val_acc: 0.2615
    Epoch 30/50
    3043/3043 [==============================] - 1s - loss: 1.8475 - acc: 0.2977 - val_loss: 2.1469 - val_acc: 0.2628
    Epoch 31/50
    3043/3043 [==============================] - 1s - loss: 1.8302 - acc: 0.3017 - val_loss: 2.1435 - val_acc: 0.2681
    Epoch 32/50
    3043/3043 [==============================] - 1s - loss: 1.8153 - acc: 0.3004 - val_loss: 2.1334 - val_acc: 0.2628
    Epoch 33/50
    3043/3043 [==============================] - 1s - loss: 1.7950 - acc: 0.3043 - val_loss: 2.1597 - val_acc: 0.2589
    Epoch 34/50
    3043/3043 [==============================] - 1s - loss: 1.7784 - acc: 0.3030 - val_loss: 2.1585 - val_acc: 0.2549
    Epoch 35/50
    3043/3043 [==============================] - 1s - loss: 1.7689 - acc: 0.3027 - val_loss: 2.1597 - val_acc: 0.2497
    Epoch 36/50
    3043/3043 [==============================] - 1s - loss: 1.7521 - acc: 0.2990 - val_loss: 2.1792 - val_acc: 0.2589
    Epoch 37/50
    3043/3043 [==============================] - 1s - loss: 1.7386 - acc: 0.3027 - val_loss: 2.1728 - val_acc: 0.2589
    Epoch 38/50
    3043/3043 [==============================] - 1s - loss: 1.7235 - acc: 0.3059 - val_loss: 2.1799 - val_acc: 0.2615
    Epoch 39/50
    3043/3043 [==============================] - 1s - loss: 1.7122 - acc: 0.3102 - val_loss: 2.1560 - val_acc: 0.2641
    Epoch 40/50
    3043/3043 [==============================] - 1s - loss: 1.6996 - acc: 0.3030 - val_loss: 2.1724 - val_acc: 0.2589
    Epoch 41/50
    3043/3043 [==============================] - 1s - loss: 1.6917 - acc: 0.3099 - val_loss: 2.1883 - val_acc: 0.2510
    Epoch 42/50
    3043/3043 [==============================] - 1s - loss: 1.6784 - acc: 0.3089 - val_loss: 2.1831 - val_acc: 0.2497
    Epoch 43/50
    3043/3043 [==============================] - 1s - loss: 1.6685 - acc: 0.3082 - val_loss: 2.1992 - val_acc: 0.2549
    Epoch 44/50
    3043/3043 [==============================] - 1s - loss: 1.6642 - acc: 0.3112 - val_loss: 2.2007 - val_acc: 0.2628
    Epoch 45/50
    3043/3043 [==============================] - 1s - loss: 1.6506 - acc: 0.3112 - val_loss: 2.2096 - val_acc: 0.2497
    Epoch 46/50
    3043/3043 [==============================] - 1s - loss: 1.6407 - acc: 0.3115 - val_loss: 2.2236 - val_acc: 0.2576
    Epoch 47/50
    3043/3043 [==============================] - 1s - loss: 1.6361 - acc: 0.3125 - val_loss: 2.2182 - val_acc: 0.2510
    Epoch 48/50
    3043/3043 [==============================] - 1s - loss: 1.6271 - acc: 0.3132 - val_loss: 2.2355 - val_acc: 0.2497
    Epoch 49/50
    3043/3043 [==============================] - 1s - loss: 1.6219 - acc: 0.3109 - val_loss: 2.2515 - val_acc: 0.2654
    Epoch 50/50
    3043/3043 [==============================] - 1s - loss: 1.6155 - acc: 0.3115 - val_loss: 2.2514 - val_acc: 0.2484
    Train on 1952 samples, validate on 489 samples
    Epoch 1/50
    1952/1952 [==============================] - 0s - loss: 3.5070 - acc: 0.0272 - val_loss: 3.3238 - val_acc: 0.0266
    Epoch 2/50
    1952/1952 [==============================] - 0s - loss: 3.2999 - acc: 0.0430 - val_loss: 3.2658 - val_acc: 0.0777
    Epoch 3/50
    1952/1952 [==============================] - 0s - loss: 3.2665 - acc: 0.0809 - val_loss: 3.2643 - val_acc: 0.0818
    Epoch 4/50
    1952/1952 [==============================] - 0s - loss: 3.2524 - acc: 0.0809 - val_loss: 3.2705 - val_acc: 0.0818
    Epoch 5/50
    1952/1952 [==============================] - 0s - loss: 3.2361 - acc: 0.0809 - val_loss: 3.2485 - val_acc: 0.0818
    Epoch 6/50
    1952/1952 [==============================] - 0s - loss: 3.2230 - acc: 0.0809 - val_loss: 3.2437 - val_acc: 0.0818
    Epoch 7/50
    1952/1952 [==============================] - 0s - loss: 3.2086 - acc: 0.0809 - val_loss: 3.2269 - val_acc: 0.0818
    Epoch 8/50
    1952/1952 [==============================] - 0s - loss: 3.1891 - acc: 0.0809 - val_loss: 3.2016 - val_acc: 0.0818
    Epoch 9/50
    1952/1952 [==============================] - 0s - loss: 3.1667 - acc: 0.0809 - val_loss: 3.1942 - val_acc: 0.0818
    Epoch 10/50
    1952/1952 [==============================] - 0s - loss: 3.1422 - acc: 0.0809 - val_loss: 3.1731 - val_acc: 0.0818
    Epoch 11/50
    1952/1952 [==============================] - 0s - loss: 3.1267 - acc: 0.0809 - val_loss: 3.1786 - val_acc: 0.0818
    Epoch 12/50
    1952/1952 [==============================] - 0s - loss: 3.1047 - acc: 0.0809 - val_loss: 3.1613 - val_acc: 0.0818
    Epoch 13/50
    1952/1952 [==============================] - 0s - loss: 3.0734 - acc: 0.0809 - val_loss: 3.1297 - val_acc: 0.0818
    Epoch 14/50
    1952/1952 [==============================] - 0s - loss: 3.0443 - acc: 0.0907 - val_loss: 3.1037 - val_acc: 0.0818
    Epoch 15/50
    1952/1952 [==============================] - 0s - loss: 2.9984 - acc: 0.1214 - val_loss: 3.0774 - val_acc: 0.1391
    Epoch 16/50
    1952/1952 [==============================] - 0s - loss: 2.9671 - acc: 0.1414 - val_loss: 3.0367 - val_acc: 0.1411
    Epoch 17/50
    1952/1952 [==============================] - 0s - loss: 2.9157 - acc: 0.1460 - val_loss: 3.0248 - val_acc: 0.1391
    Epoch 18/50
    1952/1952 [==============================] - 0s - loss: 2.8790 - acc: 0.1522 - val_loss: 2.9853 - val_acc: 0.1452
    Epoch 19/50
    1952/1952 [==============================] - 0s - loss: 2.8362 - acc: 0.1603 - val_loss: 2.9701 - val_acc: 0.1472
    Epoch 20/50
    1952/1952 [==============================] - 0s - loss: 2.7972 - acc: 0.1650 - val_loss: 2.9538 - val_acc: 0.1411
    Epoch 21/50
    1952/1952 [==============================] - 0s - loss: 2.7524 - acc: 0.1634 - val_loss: 2.9307 - val_acc: 0.1513
    Epoch 22/50
    1952/1952 [==============================] - 0s - loss: 2.7197 - acc: 0.1655 - val_loss: 2.9081 - val_acc: 0.1779
    Epoch 23/50
    1952/1952 [==============================] - 0s - loss: 2.6740 - acc: 0.1814 - val_loss: 2.8863 - val_acc: 0.1656
    Epoch 24/50
    1952/1952 [==============================] - 0s - loss: 2.6320 - acc: 0.1783 - val_loss: 2.8882 - val_acc: 0.1616
    Epoch 25/50
    1952/1952 [==============================] - 0s - loss: 2.5969 - acc: 0.1911 - val_loss: 2.8466 - val_acc: 0.1718
    Epoch 26/50
    1952/1952 [==============================] - 0s - loss: 2.5585 - acc: 0.1926 - val_loss: 2.8495 - val_acc: 0.1779
    Epoch 27/50
    1952/1952 [==============================] - 0s - loss: 2.5199 - acc: 0.1911 - val_loss: 2.8118 - val_acc: 0.1472
    Epoch 28/50
    1952/1952 [==============================] - 0s - loss: 2.4833 - acc: 0.1972 - val_loss: 2.8115 - val_acc: 0.1697
    Epoch 29/50
    1952/1952 [==============================] - 0s - loss: 2.4491 - acc: 0.2111 - val_loss: 2.7605 - val_acc: 0.1820
    Epoch 30/50
    1952/1952 [==============================] - 0s - loss: 2.4155 - acc: 0.2080 - val_loss: 2.7767 - val_acc: 0.1820
    Epoch 31/50
    1952/1952 [==============================] - 0s - loss: 2.3827 - acc: 0.2131 - val_loss: 2.7714 - val_acc: 0.1779
    Epoch 32/50
    1952/1952 [==============================] - 0s - loss: 2.3563 - acc: 0.2152 - val_loss: 2.7682 - val_acc: 0.1881
    Epoch 33/50
    1952/1952 [==============================] - 0s - loss: 2.3170 - acc: 0.2213 - val_loss: 2.7428 - val_acc: 0.1472
    Epoch 34/50
    1952/1952 [==============================] - 0s - loss: 2.2915 - acc: 0.2157 - val_loss: 2.7560 - val_acc: 0.1881
    Epoch 35/50
    1952/1952 [==============================] - 0s - loss: 2.2613 - acc: 0.2228 - val_loss: 2.7498 - val_acc: 0.1861
    Epoch 36/50
    1952/1952 [==============================] - 0s - loss: 2.2373 - acc: 0.2305 - val_loss: 2.7513 - val_acc: 0.1820
    Epoch 37/50
    1952/1952 [==============================] - 0s - loss: 2.2041 - acc: 0.2310 - val_loss: 2.7329 - val_acc: 0.1881
    Epoch 38/50
    1952/1952 [==============================] - 0s - loss: 2.1840 - acc: 0.2341 - val_loss: 2.7027 - val_acc: 0.1840
    Epoch 39/50
    1952/1952 [==============================] - 0s - loss: 2.1551 - acc: 0.2428 - val_loss: 2.6999 - val_acc: 0.1922
    Epoch 40/50
    1952/1952 [==============================] - 0s - loss: 2.1348 - acc: 0.2418 - val_loss: 2.7264 - val_acc: 0.1881
    Epoch 41/50
    1952/1952 [==============================] - 0s - loss: 2.1135 - acc: 0.2454 - val_loss: 2.6779 - val_acc: 0.2025
    Epoch 42/50
    1952/1952 [==============================] - 0s - loss: 2.0875 - acc: 0.2398 - val_loss: 2.7242 - val_acc: 0.1881
    Epoch 43/50
    1952/1952 [==============================] - 0s - loss: 2.0710 - acc: 0.2495 - val_loss: 2.7129 - val_acc: 0.1861
    Epoch 44/50
    1952/1952 [==============================] - 0s - loss: 2.0468 - acc: 0.2505 - val_loss: 2.6931 - val_acc: 0.1616
    Epoch 45/50
    1952/1952 [==============================] - 0s - loss: 2.0337 - acc: 0.2439 - val_loss: 2.7247 - val_acc: 0.1861
    Epoch 46/50
    1952/1952 [==============================] - 0s - loss: 2.0172 - acc: 0.2464 - val_loss: 2.7363 - val_acc: 0.1677
    Epoch 47/50
    1952/1952 [==============================] - 0s - loss: 1.9957 - acc: 0.2459 - val_loss: 2.7277 - val_acc: 0.1881
    Epoch 48/50
    1952/1952 [==============================] - 0s - loss: 1.9788 - acc: 0.2454 - val_loss: 2.7439 - val_acc: 0.1902
    Epoch 49/50
    1952/1952 [==============================] - 0s - loss: 1.9653 - acc: 0.2490 - val_loss: 2.7383 - val_acc: 0.1636
    Epoch 50/50
    1952/1952 [==============================] - 0s - loss: 1.9510 - acc: 0.2449 - val_loss: 2.7582 - val_acc: 0.1718
    




    <keras.callbacks.History at 0x20ee2a773c8>




## Evaluate model on test data


We do this in order to report the model accuracy on test data for each model


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

    trump score :  [1.8864540578554874, 0.1874292185561708]
    hilary score :  [1.895902678898798, 0.20858386895504824]
    obama score :  [1.8188393431467875, 0.23607805807113308]
    seinfeld score :  [1.7172567647588992, 0.30415352263911916]
    gadot score :  [2.0663390903871406, 0.24211388775702627]
    

we will define a function for getting the next word, this function will get a model and tokenizer for each famous person and give us the next word in the new tweet with random word selection from appropriate words. it will use the prev word we generated.


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

This is help function to get every time random word with good probability.


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

This function will create new tweet by using model , tokenizer and the vocabulary of common words


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

this is just a check that the function works, and gives us diffrent word each time


```python
print(get_next('Democrats', trump_tokenizer, trump_model, trump_vocab))

print(get_next('Democrats', hilary_tokenizer, hilary_model, hilary_vocab))

print(get_next('Democrats', obama_tokenizer, obama_model, obama_vocab))

print(get_next('Democrats', seinfeld_tokenizer, seinfeld_model, seinfeld_vocab))

print(get_next('Democrats', gadot_tokenizer, gadot_model, gadot_vocab))
```

    America
    thoughts
    young
    m
    League
    


```python
print(get_next('Democrats', trump_tokenizer, trump_model, trump_vocab))

print(get_next('Democrats', hilary_tokenizer, hilary_model, hilary_vocab))

print(get_next('Democrats', obama_tokenizer, obama_model, obama_vocab))

print(get_next('Democrats', seinfeld_tokenizer, seinfeld_model, seinfeld_vocab))

print(get_next('Democrats', gadot_tokenizer, gadot_model, gadot_vocab))
```

    been
    our
    act
    Any
    fans
    

Now we will create 750 new tweets, 150 for each famous, and save them in list for new tweets, list for each famous.
We also will save the famous that we use is model to report in the future of the accuracy of each algorithm in part 2.


```python
all_trump_tweets = []
all_hilary_tweets = []
all_obama_tweets = []
all_seinfeld_tweets = []
all_gadot_tweets = []
actual_tweeters = []

for i in range(0, 150):
    trump_tweet = create_sentence(trump_tokenizer, trump_model, trump_vocab)
    trump_tweet_after_stem = stem_sentence(trump_tweet)
    all_trump_tweets.extend([trump_tweet_after_stem])
    actual_tweeters.extend(['Donald J. Trump'])

for i in range(0, 150):
    hilary_tweet = create_sentence(hilary_tokenizer, hilary_model, hilary_vocab)
    hilary_tweet_after_stem = stem_sentence(hilary_tweet)
    all_hilary_tweets.extend([hilary_tweet_after_stem])
    actual_tweeters.extend(['Hillary Clinton'])
    
for i in range(0, 150):
    obama_tweet = create_sentence(obama_tokenizer, obama_model, obama_vocab)
    obama_tweet_after_stem = stem_sentence(obama_tweet)
    all_obama_tweets.extend([obama_tweet_after_stem])
    actual_tweeters.extend(['Barack Obama'])

for i in range(0, 150):
    seinfeld_tweet = create_sentence(seinfeld_tokenizer, seinfeld_model, seinfeld_vocab)
    seinfeld_tweet_after_stem = stem_sentence(seinfeld_tweet)
    all_seinfeld_tweets.extend([seinfeld_tweet_after_stem])
    actual_tweeters.extend(['Jerry Seinfeld'])

for i in range(0, 150):
    gadot_tweet = create_sentence(gadot_tokenizer, gadot_model, gadot_vocab)
    gadot_tweet_after_stem = stem_sentence(gadot_tweet)
    all_gadot_tweets.extend([gadot_tweet_after_stem])
    actual_tweeters.extend(['Gal Gadot'])

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

# PART 4

## Naiva Bayes Result

We will report the accuracy of the naive bayes algorithm, on every famous. by using the model we trained in part 2, and the tweets we generated in part 3.


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
print()
print ("We Succeded in: " + str(((gadot_right+seinfeld_right+trump_right+hilary_right+obama_right) /750)*100) + " precent in prediction which tweets are of the right person ")
```

    We Succeded in: 70.66666666666667 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 38.666666666666664 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 48.0 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 34.0 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 10.0 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    
    We Succeded in: 40.266666666666666 precent in prediction which tweets are of the right person 
    

## We can see above that The Naive Bayes algorithm is best recognizing Trump Tweets and Worst recognizing Gal Gadot Tweets. This is probably because Trump uses more unique words so his tweets can be recognized easily.

We will save the predictions of naive bayes on each one of the 750 new tweets, in order to plot confussion matrix.


```python
nb_predict_tweeters = []

for i in range(0, 150):
    nb_predict_tweeters.extend(nb.predict([all_trump_tweets_BOW[i]]))

for i in range(0, 150):
    nb_predict_tweeters.extend(nb.predict([all_hilary_tweets_BOW[i]]))

for i in range(0, 150):
    nb_predict_tweeters.extend(nb.predict([all_obama_tweets_BOW[i]]))
    
for i in range(0, 150):
    nb_predict_tweeters.extend(nb.predict([all_seinfeld_tweets_BOW[i]]))
    
for i in range(0, 150):
    nb_predict_tweeters.extend(nb.predict([all_gadot_tweets_BOW[i]]))
```

Import libraries for ploting the confussion matrix. 


```python
import itertools
import numpy as np
import matplotlib.pyplot as plt

from sklearn import svm, datasets
from sklearn.model_selection import train_test_split
from sklearn.metrics import confusion_matrix
```

This is function for ploting the confussion matrix.

This will plot confussion matrix for naive bayes algorithm.


```python
print(__doc__)

class_names = ['Donald J. Trump', 'Hillary Clinton', 'Barack Obama', 'Jerry Seinfeld' ,'Gal Gadot' ]

def plot_confusion_matrix(cm, classes,
                          normalize=False,
                          title='Confusion matrix',
                          cmap=plt.cm.Blues):
    """
    This function prints and plots the confusion matrix.
    Normalization can be applied by setting `normalize=True`.
    """
    if normalize:
        cm = cm.astype('float') / cm.sum(axis=1)[:, np.newaxis]
        print("Normalized confusion matrix")
    else:
        print('Confusion matrix, without normalization')

    print(cm)

    plt.imshow(cm, interpolation='nearest', cmap=cmap)
    plt.title(title)
    plt.colorbar()
    tick_marks = np.arange(len(classes))
    plt.xticks(tick_marks, classes, rotation=45)
    plt.yticks(tick_marks, classes)

    fmt = '.2f' if normalize else 'd'
    thresh = cm.max() / 2.
    for i, j in itertools.product(range(cm.shape[0]), range(cm.shape[1])):
        plt.text(j, i, format(cm[i, j], fmt),
                 horizontalalignment="center",
                 color="white" if cm[i, j] > thresh else "black")

    plt.tight_layout()
    plt.ylabel('True label')
    plt.xlabel('Predicted label')

# Compute confusion matrix
cnf_matrix = confusion_matrix(actual_tweeters, nb_predict_tweeters)
np.set_printoptions(precision=2)

# Plot non-normalized confusion matrix
plt.figure()
plot_confusion_matrix(cnf_matrix, classes=class_names,
                      title='Confusion matrix, without normalization')

plt.show()
```

    Automatically created module for IPython interactive environment
    Confusion matrix, without normalization
    [[ 72  39   5  18  16]
     [ 19 106   4  12   9]
     [ 18  72  15  31  14]
     [ 56  27   2  58   7]
     [ 42  21  13  23  51]]
    


![Image of github's cat](/images/photo2.png)


## Logistic Regression Result

We will report the accuracy of the Logistic Regression algorithm, on every famous. by using the model we trained in part 2, and the tweets we generated in part 3.


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
print()
print ("We Succeded in: " + str(((gadot_right+seinfeld_right+trump_right+hilary_right+obama_right) /750)*100) + " precent in prediction which tweets are of the right person ")
```

    We Succeded in: 37.333333333333336 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 42.0 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 46.666666666666664 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 44.0 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 41.333333333333336 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    
    We Succeded in: 42.266666666666666 precent in prediction which tweets are of the right person 
    

## We can see above that The Regression algorithm is best recognizing all the persons tweets in the same probability( more or less) . and with this tweets this algorithm give the best results from all the others.


We will save the predictions of Logistic Regression on each one of the 750 new tweets, in order to plot confussion matrix.


```python
regresion_predict_tweeters = []

for i in range(0, 150):
    regresion_predict_tweeters.extend(regresion.predict([all_trump_tweets_BOW[i]]))

for i in range(0, 150):
    regresion_predict_tweeters.extend(regresion.predict([all_hilary_tweets_BOW[i]]))

for i in range(0, 150):
    regresion_predict_tweeters.extend(regresion.predict([all_obama_tweets_BOW[i]]))
    
for i in range(0, 150):
    regresion_predict_tweeters.extend(regresion.predict([all_seinfeld_tweets_BOW[i]]))
    
for i in range(0, 150):
    regresion_predict_tweeters.extend(regresion.predict([all_gadot_tweets_BOW[i]]))
```

This will plot confussion matrix for the logistic regression algorithm.


```python
cnf_matrix = confusion_matrix(actual_tweeters, regresion_predict_tweeters)
np.set_printoptions(precision=2)

# Plot non-normalized confusion matrix
plt.figure()
plot_confusion_matrix(cnf_matrix, classes=class_names,
                      title='Confusion matrix, without normalization')

plt.show()
```

    Confusion matrix, without normalization
    [[70 17 32 12 19]
     [12 56 44 20 18]
     [22 18 62 32 16]
     [34  9 35 63  9]
     [12  5 50 17 66]]
    


![Image of github's cat](/images/photo3.png)


## SVC Result

We will report the accuracy of the SVC algorithm, on every famous. by using the model we trained in part 2, and the tweets we generated in part 3.


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
print()
print ("We Succeded in: " + str(((gadot_right+seinfeld_right+trump_right+hilary_right+obama_right) /750)*100) + " precent in prediction which tweets are of the right person ")
```

    We Succeded in: 23.333333333333332 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 37.333333333333336 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 50.66666666666667 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 49.333333333333336 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 38.0 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    
    We Succeded in: 39.733333333333334 precent in prediction which tweets are of the right person 
    

## We can see above that The SVC algorithm is best recognizing Obama Tweets and Worst recognizing Trump Tweets. 

We will save the predictions of SVC on each one of the 750 new tweets, in order to plot confussion matrix.


```python
svc_predict_tweeters = []

for i in range(0, 150):
    svc_predict_tweeters.extend(svc.predict([all_trump_tweets_BOW[i]]))

for i in range(0, 150):
    svc_predict_tweeters.extend(svc.predict([all_hilary_tweets_BOW[i]]))

for i in range(0, 150):
    svc_predict_tweeters.extend(svc.predict([all_obama_tweets_BOW[i]]))
    
for i in range(0, 150):
    svc_predict_tweeters.extend(svc.predict([all_seinfeld_tweets_BOW[i]]))
    
for i in range(0, 150):
    svc_predict_tweeters.extend(svc.predict([all_gadot_tweets_BOW[i]]))
```

This will plot confussion matrix for the SVC algorithm.


```python
cnf_matrix = confusion_matrix(actual_tweeters, svc_predict_tweeters)
np.set_printoptions(precision=2)

# Plot non-normalized confusion matrix
plt.figure()
plot_confusion_matrix(cnf_matrix, classes=class_names,
                      title='Confusion matrix, without normalization')

plt.show()
```

    Confusion matrix, without normalization
    [[76 12 42 10 10]
     [19 35 58 20 18]
     [30 16 57 32 15]
     [47  4 33 56 10]
     [14  6 41 15 74]]
    


![Image of github's cat](/images/photo4.png)


## KNN Result

We will report the accuracy of the KNN algorithm, on every famous. by using the model we trained in part 2, and the tweets we generated in part 3.


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
print()
print ("We Succeded in: " + str(((gadot_right+seinfeld_right+trump_right+hilary_right+obama_right) /750)*100) + " precent in prediction which tweets are of the right person ")
```

    We Succeded in: 0.0 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 0.0 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 0.0 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 0.0 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 100.0 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    
    We Succeded in: 20.0 precent in prediction which tweets are of the right person 
    

## We can see that this algorithm always chooses Gal Gadot as the person who tweeted. And is the worst from all the algorithms.

We will save the predictions of KNN on each one of the 750 new tweets, in order to plot confussion matrix.


```python
knn_predict_tweeters = []

for i in range(0, 150):
    knn_predict_tweeters.extend(knn.predict([all_trump_tweets_BOW[i]]))

for i in range(0, 150):
    knn_predict_tweeters.extend(knn.predict([all_hilary_tweets_BOW[i]]))

for i in range(0, 150):
    knn_predict_tweeters.extend(knn.predict([all_obama_tweets_BOW[i]]))
    
for i in range(0, 150):
    knn_predict_tweeters.extend(knn.predict([all_seinfeld_tweets_BOW[i]]))
    
for i in range(0, 150):
    knn_predict_tweeters.extend(knn.predict([all_gadot_tweets_BOW[i]]))
```

This will plot confussion matrix for the KNN algorithm.


```python
cnf_matrix = confusion_matrix(actual_tweeters, knn_predict_tweeters)
np.set_printoptions(precision=2)

# Plot non-normalized confusion matrix
plt.figure()
plot_confusion_matrix(cnf_matrix, classes=class_names,
                      title='Confusion matrix, without normalization')

plt.show()
```

    Confusion matrix, without normalization
    [[  0   0 150   0   0]
     [  0   0 150   0   0]
     [  0   0 150   0   0]
     [  0   0 150   0   0]
     [  0   0 150   0   0]]
    


![Image of github's cat](/images/photo5.png)


## Random Forest Result

We will report the accuracy of the Random Forest algorithm, on every famous. by using the model we trained in part 2, and the tweets we generated in part 3.


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
print()
print ("We Succeded in: " + str(((gadot_right+seinfeld_right+trump_right+hilary_right+obama_right) /750)*100) + " precent in prediction which tweets are of the right person ")
```

    We Succeded in: 14.666666666666666 precent in prediction which tweets are of Trump from the tweets we generated for him
    We Succeded in: 30.0 precent in prediction which tweets are of Hilary from the tweets we generated for her
    We Succeded in: 22.0 precent in prediction which tweets are of obama from the tweets we generated for him
    We Succeded in: 26.0 precent in prediction which tweets are of Seinfeld from the tweets we generated for him
    We Succeded in: 75.33333333333333 precent in prediction which tweets are of Gal Gadot from the tweets we generated for her
    
    We Succeded in: 33.6 precent in prediction which tweets are of the right person 
    

## We can see above that The Random Forest algorithm is best recognizing Gal Gadot Tweets and Worst recognizing Trump Tweets. 

We will save the predictions of Random Forest on each one of the 750 new tweets, in order to plot confussion matrix.


```python
forest_predict_tweeters = []

for i in range(0, 150):
    forest_predict_tweeters.extend(forest.predict([all_trump_tweets_BOW[i]]))

for i in range(0, 150):
    forest_predict_tweeters.extend(forest.predict([all_hilary_tweets_BOW[i]]))

for i in range(0, 150):
    forest_predict_tweeters.extend(forest.predict([all_obama_tweets_BOW[i]]))
    
for i in range(0, 150):
    forest_predict_tweeters.extend(forest.predict([all_seinfeld_tweets_BOW[i]]))
    
for i in range(0, 150):
    forest_predict_tweeters.extend(forest.predict([all_gadot_tweets_BOW[i]]))
```

This will plot confussion matrix for the Random Forest algorithm.


```python
cnf_matrix = confusion_matrix(actual_tweeters, forest_predict_tweeters)
np.set_printoptions(precision=2)

# Plot non-normalized confusion matrix
plt.figure()
plot_confusion_matrix(cnf_matrix, classes=class_names,
                      title='Confusion matrix, without normalization')

plt.show()
```

    Confusion matrix, without normalization
    [[ 33   3  90  14  10]
     [  4  22 106  14   4]
     [ 14   0 113  21   2]
     [ 14   0  80  45  11]
     [  6   0  94  11  39]]
    


![Image of github's cat](/images/photo6.png)


## conclusions


### most of our conclusions are detailed above. but we think we can improve the results in part 4, if we would build the sentence maybe with other algorithms than in Recurrent Neural Network, and maybe if instead of building the sentence his random words choosing we would do it by choosing the most impropriate word in each step.


### In our algorithm of choosing random word in each step when building the sentence we saw that the regression algorithm is the best classifier for the tweets we built and the knn is the worst.
