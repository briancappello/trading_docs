# An Absurdly Brief Introduction to Trading, Backtesting and Algorithm Analysis

In this document we're going to introduce some of the considerations you'll need to take into account while developing and optimizing your algorithmic trading systems. First, we'll touch on developing systems compatible with your personality and your strongest understandings of market dynamics. Secondly, we'll give a heavily abbreviated [and admittedly biased] overview of the history of trading, as well as some of the classifications of various types of trading strategies. Finally, and **perhaps most importantly, we'll dig into the factors you should take into account while analyzing the "validity and trusability" of the systems you're developing**. Just because a system is "backtested" tells you nothing on its own - you need to not only fully understand how to *properly* backtest, but also how to thoroughly analyze the *implications* of the results.

> Edwin Lefèvre/Jesse Lauriston Livermore, *Reminiscences of a Stock Operator*

> **Professional traders have always had some system or other based upon their experience and governed either by their attitude toward speculation** or by their desires. I remember I met an old gentleman in Palm Beach whose name I did not catch or did not at once identify. I knew he had been in the Street for years, way back in Civil War times, and somebody told me that he was a very wise old codger who had gone through so many booms and panics that he was always saying **there was nothing new under the sun and least of all in the stock market.**

Jack Schwager, futures trader and author of the highly acclaimed *Marked Wizards* series, had this to say in the introduction of his second book in the series, *New Market Wizards*:

> 1. The markets are not random. I don't care if the number of academicians who have argued the efficient market hypothesis would stretch to the moon and back if laid end to end; they are simply wrong. <small>(There are three variations of the efficient market hypothesis: (1) *weak form*: past prices cannot be used to predict future prices; (2) *semistrong form*: the current price reflects all publicly known information; and (3) *strong form*: the current price reflects all information, whether publicly known or not.)</small>

> 2. The markets are not random, because they are based on human behavior, and human behavior, especially mass behavior, is not random. It never has been, and it probably never will be.

> 3. There is no holy grail or grand secret to the markets, but there are many patterns that can lead to profits.

> 4. There are a million ways to make money in markets. The irony is that they are all very difficult to find.

> 5. The markets are always changing, and they are always the same.

> 6. **The secret to success in the markets lies not in discovering some incredible indicator or elaborate theory; rather, it lies within each individual.**

> 7. To excel in trading requires a combination of talent and extremely hard work - (surprise!) the same combination required for excellence in any field. Those seeking success by buying the latest $300 or even $3,000 system, or by following the latest hot tip, will never find the answer because they haven't yet understood the question.

> 8. Success in trading is a worthy goal, but it will be worthless if it is not accompanied by success in your life (and I use the word success here without monetary connotation).

> In conducting the interviews for this book and its predecessor. Market Wizards, I became absolutely convinced that winning in the markets is a matter of skill and discipline, not luck. The magnitude and consistency of the winning track records compiled by many of those I interviewed simply defy chance. I believe the Market Wizards provide role models for what it takes to win in the markets. **Those seeking quick fortunes should be discouraged at the onset.**


While various aspects of the following discussion apply to all types of financial markets, for the sake of brevity **this document will focus primarily on exchange- and ECN-traded equities markets**. (More commonly known as "the stock market".) It should be understood, however, that equities make up only a small fraction (by both volume *and* diversity) of the world's tradable financial instruments. While overlap is inevitable, especially amongst the quants and their derivatives-pricing models, we will largely ignore foreign exchange, options, futures and commodities, corporate bonds and warrants [convertable or not], and government bonds. And that's not to even mention all of the other hybrid instruments and derivatives financial institutions have pulled out of their hats to meet various customers' needs (at the end of the day, *all* financial instruments are "simply" tools for distributing risk from one party to another).

Furthermore, due to technological limitations, we will not discuss high-frequency trading strategies, where high-frequency is defined as roundtrip trade times of less than one minute. Depending upon who you ask throughout various parts of the industry, high-frequency trading can refer to anything from sub-second holding periods to trades that are held for an hour or more. It's all a matter of relativity. 


## Words of Warning

Edwin Lefèvre/Jesse Lauriston Livermore, *Reminiscences of a Stock Operator*:

> **Of course the same things happen in all speculative markets. The message of the tape is the same.** That will be perfectly plain to anyone who will take the trouble to think. He will find if he asks himself questions and considers conditions, that the answers will supply themselves directly. But people never take the trouble to ask questions, leave alone seeking answers. The average American is from Missouri everywhere and at all times except when he goes to the brokers' offices and looks at the tape, whether it is stocks or commodities. **The one game of all games that really requires study before making a play is the one he goes into without his usual highly intelligent preliminary and precautionary doubts. He will risk half his fortune in the stock market with less reflection than he devotes to the selection of a medium-priced automobile.**

> ... Nobody should be puzzled as to whether a market is a bull or a bear market after it fairly starts. The trend is evident to a man who has an open mind and reasonably clear sight, for **it is never wise for a speculator to fit his facts to his theories.** Such a man will, or ought to, know whether it is a bull or a bear market, and if he knows that he knows whether to buy or to sell. **It is therefore at the very inception of the [price] movement that a man needs to know whether to buy or to sell.**


Monroe Trout, in *New Market Wizards*:

> **What kinds of misconceptions do people have about the markets?**

> They believe you can make tons of money with little work. They think you can make 100 percent a year doing a little bit of research on the weekends. That's ridiculous.

> **They underestimate the difficulty of the game and overestimate the payoff?**

> Exactly. Also, some people blame everyone except themselves when they lose money. It galled me to read in a recent *Wall Street Journal* article that some guy actually won a lawsuit against his brokerage firm because he lost all the money in his account. The point is that it wasn't even a matter of his broker giving him bad advice; he was calling his own trades! He sued the brokerage firm, saying that they shouldn't have allowed him to trade his account the way he did. **I believe it's a free country, and if you want to trade, you should have every right to do so, but if you lose money, it's your own responsibility.**

> **What mistakes do most people make in the markets? I'm talking about actual trading mistakes rather than misconceptions.**

> First, many people get involved in the markets without any edge. They get in the market because their broker told them that the market is bullish. That is not an edge. However, **to tell the truth, most small speculators will never be around long enough to find out whether their system could have worked, because they bet too much on their trades, or their account is too small to start.**


Marcel Link, *High Probability Trading*:

> Many people like to find fault elsewhere. When you [or your systems] are trading badly, it is your fault and no one else's. It's not the market maker's, a bad quote's, a broker's, a local's, or a computer problem's fault. **There is no "they" that is out to get you.** If you lost money or got a bad fill, it's your fault; don't blame anyone else. One has to accept responsibility for one's mistakes. If a broker misleads you, it's your fault for listening to him; at the least the second time it is. Don't blame other people or other things for your bad trading [or bad algorithms]. If you do, you will never get better because you won't be working on correcting your weaknesses, as you won't think you have any. If you want to be successful, you will accept the blame for losses and not pass it off to the market, a broker, or a bad fill.


## The Imperfect Human Being

Humans are primarily irrational, highly emotional beings - though not random; behavioral economics proves there's predictability in irrationality. Some people are less rational than others, of course, but the fact remains that most of us under pressure are not very good at making well-reasoned decisions; especially when our brokerage accounts and reputations - egos - are on the line.

> <small>John Carter, *Mastering the Trade*</small>

> **The best lesson I've ever learned about short-term trading** happened while on a white water rafting trip. Eight of us were in the raft when it hit a rock and flipped, launching us into the air like a catapult, sending everyone headfirst into the icy water. Half of us remembered that, in the event of a spill, to stay calm and position ourselves on our back, feet facing downstream. We zipped around rocks and through cascades of water, eventually dragging ourselves safely ashore. An hour passed before we learned what happened to the rest of the group. For them, a rescue operation went into effect, and the end result was a gashed leg, a concussion, and a near drowning. Later, when speaking to the abused, I learned that all of them had experienced a type of brain freeze. They could see the danger around them. They knew they were in trouble. **They even knew they needed to act, to do something. But they literally could not make a decision about what action they should take.** So, they took the only option left to them: They froze like the proverbial deer in the headlights and did nothing. In the absence of a decisive path of action, the river grabbed them by their lapels and, like an angry pimp with bills to pay, slapped them senseless.

> I remember one member of the group saying, "That river was out to get me!" Extreme paranoia and self-centeredness aside, the river was not out to get anyone. It did what it was supposed to do; move quickly and rapidly through a canyon in order to get to the ocean. **The riders who understood the nature of the river were prepared and took the roller coaster journey in stride. The riders who were unprepared got thrashed.**

> The similarities between this event and a typical trading day are nearly identical. The unprepared trader is in the same situation as the unprepared white water rafter. In the even of extreme conditions, both will freeze, and both will be lucky to survive the experience. **One bad trade can wipe out months or years of profits.**

In theory, computers should be able to solve these silly human weaknesses for us - for the time being, computers don't have emotions to cloud their judgment. Never the less, everything's not all peaches and cream. **Even with computers in the driver's seat, at the end of the day, it's *still your code, your money, and your ego on the line*.** Are you trading to put food on the table, or just playing around with excess monies? If you're depending on profits to support your family and/or life responsibilities, personally, I would recommend you take a hard look at your situation and understand that **trading with emotionalized money is a proven extremely dangerous proposition.**

> <small>Marcel Link, *High Probability Trading*</small>

> **Rule number one of any system is to make sure you are comfortable using it and believe in the signals it generates.** To do this you need to know what kind of trader you are and what makes you tick; you need to analyze your personality and figure out how you want to trade. Are you a person who is in it for the excitement or to make money? Would you be happy trading once a week if you knew you'd make money or do you need to trade 50 times a day? Do you like to trade reversals, trends, or breakouts, or do you have another preference? ... No matter what type of trader you are, you will work better when you are comfortable with a system that complements your beliefs.

Outside of understanding your personality and how that relates to the strategies most congruent with yourself, there are further practical considerations you must also take into account. Namely, how much trading capital do you have? The larger your account, the wider the range of available system methodology options you'll have. This is due to the fixed and variable costs of trading - commission and slippage, respectively. These costs will eat you alive if you let your trading frequency get out of hand relative to your account balance and average profits. As a general rule, the quicker and more often you trade in and out of the markets, the greater the size you'll need to offset these fixed costs.

> <small>Marcel Link, *High Probability Trading*</small>

> A fellow trader told me early on as he saw me overtrading to concentrate on *preserving precious capital.* He used to write "PPC" on the top of his trading pads to remind himself to preserve precious capital. He said, **"Forget about making money; just try as hard as possible not to lose any. Every dollar you have is precious, and fight as hard as possible to keep it in your pocket and out of someone else's."** If you keep making smart trades and preserving your money, you'll be standing long after most other traders, and that gives you a greater chance to win. **The key to being a winning trader is to not lose a lot when you lose.** If you cut losses, the winning trades will take care of themselves.

The point here is that "paper losses" are, in actuality, very real losses. When the market has moved against your position, there is nothing which suggests the market will magically turn again and put your position back in the black. Sure, that may sometimes indeed happen, but *counting* on it to happen is an extremely foolish proposition. While you need to give your entries room to breathe - setting your stops too tightly is also a mistake - there is absolutely no better way to flush your entire trading account down the toilet than to hold on to a loser, hoping for luck to fall from the moon. The markets are a merciless place; hope has no seat at the table here.

# Broad Classifications of Trading System Types

Before we get into specifics, it's important to recognize that there are literally millions of ways to make money in the markets. None of them are even remotely easy to discover. This discovery problem has only been exacerbated by the enourmous influx of professionals into the markets since the 1960s and 70s. Broadly speaking, there are three major methods of analyzing markets. By far the oldest method is known as technical analysis (TA). Sometime during the mid 18th century the Japanese began documenting their methods of TA, well before the NYSE was founded in 1792. (Their specific methods remained unknown to the western world until Steve Nison's *Japanese Candlestick Charting Techniques* came onto the scene in 1991.) Western TA developed independently of the Japanese's methods, and it was not until the 1880s and 1890s that various western practitioners began to document their strategies. While the eastern and western approaches to TA appear quite different on the surface, fundamentally, they are compatible and complementary tools for guaging changes in price direction due to shifts in market participants' sentiments as printed in the price action itself. The second-oldest method is known as fundamental analysis - a method originating in the 1930s based upon the premise that prices are [or should be] based upon underlying economic factors. Finally, since the advent of computers the newest approach has come to be known as quantitative analysis.

### Fundamentalists vs Technicalists

> <small>Justin and Robert Mamis, *When to Sell*</small>

> **The premise of technical analysis is that the market, as a game, yields to the study of risk on a game-theory basis; that is, from the market's action itself.** Indeed, it has always seemed to us that fundamentalists, with their scrutiny of corporate balance sheets, cash flows, and economic conditions, have much the harder row to hoe, with far less chance of being right. **One of the single most important distinctions to understand about the market is that you are never buying or selling a company; you are bidding and offering in an auction.** Prices are not determined by boards of directors, by an illustrious company name, or by an exotic product line, but soley by whatever amount someone is willing to pay or accept for that stock at any given moment.

> Now, of course, the real person behind those orders may have based his decision on the grounds that the earnings associated with that particular ticker symbol are desirable, to this way of thinking; if so, the market action itself will tell us that someone wants to buy those shares. **It is far more important for us to know *what* such buying interest is aiming for, and *how* powerful it is, than to know precisely *why* the buying is forthcoming.** The market is the sum of everything that everyone knows about every company - and acts upon. So the buying and selling forces are all there, summarized by the market's own internal data.

> That is not to say fundamentals don't have their place; they do, but it's a small place. Fundamentals are useful, for example, when, after already having a sense of the market's future technically, you want to decide which of two companies to buy. Basically, fundamentals are the concern of businesses themselves and economists. ... If a fundamentalist approach worked on its own, an item such as the price/earnings ratio, cherished by those analysts who must justify their desk space and carpet on the floor, would be an accurate basis for buying and selling. But a glance backward shows how undependable it is; what used to be valued at ten times earnings in 1973 become a huge loss in 1974 as the same stock slipped to sell at five or four or even two times earnings. At other times a stock which seems overvalued at thirty times earnings shoots up to one hundred times earnings before fading. Because so much of the market depends on mood, no fundamental analysis has predictive value.

> It is, therefore, the fundamentalists who are playing the dangerous game. Technicians, on the other hand, want to know such objective data as whether there are more sellers than buyers, how strong each competing side is, and who - professional or odd-lotter - is on which side. Then we know how to play the game with the least risk.

> The technical approach is based on whatever the market environment itself discloses: more sellers than buyers, optimistic odd-lotters, fewer stocks making new highs. **Behind the various indicators is the consensus of those who presumably know about value, the intricaces of the Federal Reserve monetary policy, the prospects of a new product, etc. Collectively they have the strength to exercise an influence on stock prices. In order to profit from their knowledge, they have to act in the marketplace by buying or selling, and whatever conclusions their opinions bring to the stock market become technical evidence.** Similarly, other indicators measure the emotional responses of amateurs, who have a track record of being wrong when it counts. Thus, in a manner of speaking, the technical analyst has the benefit of Wall Street's best thinkers at his beck and call and can also determine which advice and activities to stay away from.

### Technicalists vs Quants

> <small>Emanual Derman, *My Life as a Quant*</small>

> **Traders and quants are genuinely different species.** Traders pride themselves on being tough and forthright while quants are more circumspect and reticent. These differences in personality are reflections of deeper cultural preferences. Traders are paid to act. All day long they watch screens, assimilate economic information, page frantically through spreadsheets, run programs written by quants, enter trades, talk to salespeople and brokers, and punch keys. It's hard to have an extended conversation with a trader during the business day; it takes an hour of standing around to have five minutes of punctuated repartee. Part of what traders do has a video game quality. In consequence, they learn to be opinionated, visceral, fast-thinking, and decisive, though not always right. They thrive on interruption.

> Quants do not. Like academics training in research, they prefer to do one thing from the beginning to end, deeply and well. This is a luxury that is difficult to enjoy in the multitasking world of business, where you have to do many things simultaneously. When I moved to Wall street, the hardest attitude adjustment for me was to learn to carry out multiple assignments in parallel, to interrupt one urgent and still incomplete task in another more pressing one, to complete that, and then pop the stack.

> Traders and quants think differently, too. **Good traders must be perpetually aware of the threat of change and what it will do to the value of their positions.** Stock options in particular, because of their intrinsic asymmetry, magnify stock price changes and therefore suffer or benefit dramatically from even small moves. **Quants think less about future change and more about current value.** According to financial theory, at any instant the so-called fair value of a security is an average over the range of all its possible future values. Fair value and change are therefore two sides of the same coin; the more ways in which a security can lose value from a future market move, the less it should rationally be worth today, and hence the mantra: more risk, more return. This difference between the quant's view of value as an average versus the trader's need to worry about any change makes this kind of professional cross-communication difficult.

> Tour de France cyclists don't need to know how to solve Newton's Laws in order to bank around a curve. Indeed, thinking too much about physics while cycling may prove a hindrance. Similarly, options traders need not be expert quants; they can leave the details of the [pricing] recipe for manufacturing options to others as long as they have the patience to thoroughly understand how to use it and when to trust it, for **no model is perfect**. One trader I know used to say, "You can't give a person a Black-Scholes calculator and turn him into a trader," and this is true; **it takes study, understanding, intuition, and grasp of the limits of the model in order to trade wisely. You cannot just follow formulas, no matter how precise they appear to be.**

> **A good quant must be a mixture, too - part trader, part salesperson, part programmer, and part mathematician.** Many quants would like to cross over to become traders, but they face the formidable obstacles of scholarly backgrounds, introspective personalities, and hybrid styles.

There are numerous examples of mind-numbing success stories from all three camps of market analysis styles. While you barely ever hear about the losers - aside from the most spectacular examples - rest assured these stories also come from all three camps, and by all rights dwarf the number of success stories in stunning quantities. For one, who wants to read somebody else's pitty story, and more tellingly, if there were no fresh blood coming into the markets on a daily basis, there'd be less "easy" profits on the table for the taking by those who've done their research, paid their tuitions to the markets and made it into the winner's bracket. (*Staying* in the winner's bracket is another matter entirely.)

Once again, the biggest point to take away from this discussion is the utmost importance of taking a deep look at your personality to decipher the methods with which you personally are most comfortable using. Nobody else can tell you "the magic formula" or what strategies will work best for you - they can only share what does or doesn't work for themselves. (If they even decide to divulge such information in the first place.)


### Understanding the Building Blocks of Technical Analysis

> <small>Bruce Kamich, *How Technical Analysis Works*</small>

> **Support and resistance areas form because market participants remember price levels, and they tend to react as a group when a stock returns to those prices.** A simple example will make the concept easy to understand. Let's imagine that you and other investors bought a stock at its initial public offering price of $20. People who did not buy the stock at the offering price are interested in buying it at that level, and they buy the stock at $20 or perhaps above $20 as interest in the stock builds. Other buyers come in and the stock trades up to $22.

> The stock may trade back and forth between $20 and $22, but let's imagine that at some point the stock slips down to $16. Some traders will hold on to the stock, hoping that its fortunes will reverse and it will trade back above $22. But if the stock remains depressed, owners of the stock who bought it in the $20 to $22 area will begin to think it would be nice just to break even. If the stock does trade back up, the desire to get out at break-even will become even stronger as the stock approaches the $20 to $22 area.

> To put this price zone into perspective, the more sideways trading that occurs, the greater the supply of stock will be. There will be more people who want to "get even," and therefore there will be more resistance to the stock's advance.

> **Support and resistance are *not* precise concepts.** When support develops during a decline at a price short of the exact level, it is usually because traders are anxious. They remember the prior resistance level at $22, but they start buying on the way down at $22.75 and $22.50 because they are anxious or even fearful they will not have the opportunity to buy again at $22.

> On a few [now much more often] occasions, eager traders might push a market too quickly through a support level, and support might not develop until just beyond the support area[, oftentimes because professionals wanted to wipe out poorly placed stops.] **Whether support is found short of the expected level or just beyond the level will tell you how eager or fearful traders are.**

> We have seen that when support is broken on the downside, it then becomes resistance. The opposite is also true; resistance, once broken, becomes support. This reversal of roles is due to the memory of traders and investors who want to get out of their losing trades at break-even, and traders who want to increase winning positions by buying more stock at or near support.

> **The role reversal of support and resistance leads to the formation of trends,** because in an uptrend, market pullbacks or reactions will tend to find support at the last resistance level. In a downtrend, reactions or rallies will tend to find resistance at the last support level.

> **All support and resistance levels are *not* equal.** The strength of a support or resistance level depends on several factors, such as the number of times the level or area was tested, the volume of trades transacted there, how long ago the formation appeared, and whether it was a round number of a "big figure." Even knowing the type of security will help in determining the validity of the support or resistance area.

> **The more times a level or zone of prices is tested, the more important that level becomes.** ...A level that is tested six times and holds tends to be more important that one that was tested only twice. The more times a support or resistance level is tested, the more traders will remember it and the more traders will be likely to be committed near it.

> **When a large volume of trading occurs in a support or resistance area, that adds to its validity because a greater number of traders and investors will remember the level, so their commitment to the level will likely be greater.**

> **The further back in time the support or resistance area was formed, the dimmer the memory, and the more likely that people have already acted on new information and may not respond in the same way again.** They moved on by liquidating their positions. An area of support or resistance that was formed recently tends to attract a greater number of people who are still committed to the level. Thus, these nearby levels have more validity or potency.

> **When a support or resistance level forms at a round number or a big figure, such as $100 or $1,000 or DJIA 10,000, many more people will remember the level because it is easy to remember and obvious.** In turn, the more people who remember and act at that level, the stronger the support or resistance will be.


***By many accounts, the rules of the 21st century trading game have entirely changed.*** This is thanks primarily to the decreasing percentage of humans making trading decisions relative to computer algorithms. (In the equities markets, 2010 estimates suggest less than 5% of trades have a human directly behind them.) Revolutionary advances in mathematics for the pricing of derivatives since the 1970s, alongside competative advances in automated order matching (ECNs) combined with favorable regulatory changes are some of the primary reasons for this computerized shift. Such statements of changed rules primarily apply to the shortest time frames of market dynamics - we're talking about sub-second high-frequency trades. These short-term dynamics differ from larger market moves in much the same way subatomic physics bears little resemblance to the Newtonian physics of the world we're capable of observing with our own eyes.

When the sailing is smooth, you can more or less tack on a few extra points of slippage to your transaction costs and look the other way. Except, of course, when something goes horribly wrong: it is of paramount importance to be aware of the extreme manipulative power these HFT systems can hold over the markets. ***When freak "acts of god" occur or some of the HFT models' shit slips through the cracks and hits the fan, it happens violently and with blistering speed***. If you see something happening *across the markets* that you don't understand, GET OUT. The reason why can wait; your account can't. [Of course, it is this very advice which helps exacerbate flash-crashes, but that's not our primary problem to deal with, is it?]


# 20th Century Strategies: Breakouts, Trends and Oscillator-Based Systems

These systems are the creation of the technical analysts. Depending upon how far back in time you go, such people are often referred to simply as speculators, tape-readers or [black art] chartists. By today's standards, these systems are all longer-term systems - falling somewhere between day trading, swing trading and not quite buy-and-hold investing. It is essential to understand the reasoning behind these systems: they are grounded in the notion that *markets are inherently inefficient because human fear, greed and herd psychology are greater forces of market movements than the notion of humans as rational decision makers.*

> <small>Marcel Link, *High Probability Trading*</small>

> I recommend that anyone who hasn't read Charles Mackay's *Extraordinary Popular Delusions and the Madness of Crowds* read it. Next to *Reminiscences of a Stock Operator*, it is the most highly recommended book for traders to read. It highlights several incidents throughout time when a fool and his money were parted because of greed. The most famous is the great tulip craze of the 1600s. People bought flowers simply because prices were going higher and they wanted to cash in. The market was flying higher everyday with price and realistic valuations not a concern because as long as the market rallied, there was money to be made.

> The speculative fire was fueled by people getting into debt just to get involved. Prices soared almost 6000 percent in just 3 years. Then, all of a sudden, *boom!* They collapsed, falling 90 percent in only 3 months, never to bounce back. The economy went into a tailspin as heavy debt caused bankruptcies to soar. Sounds familiar to many NASDAQ traders in 2000, but it's what took place in the 1630s as the frenzy for tulips took Holland by storm. It just goes to show that times and people don't really change. Any time there is a chance of making a buck, greed will prevail.

The lesson to be learned here is that **market price distributions cannot be modeled by normal distribution bell curves**. On the contrary, market price behavior fits into fat-tailed distribution curves - in other words, **extreme events are far more common than classical statistics would suggest**. You'll have noticed that large swaths of market participants *still* have not headed this warning - leading to worse and worse "largest crash in Wall Street history" days. Stated more explicitly, if you wish to survive to trade for tomorrow, always over-estimate unknown risks and be extremely conservative with leverage - if you even decide to use it at all.

> <small>William Eckhardt, in *New Market Wizards*</small>

> William Eckhardt is one of the key figures in a famous financial tale, yet he is virtually unknown to the public. If elite traders were as familiar as leading individuals in other fields, one could picture Eckhardt appearing in one of those old American Express ads (which featured famous yet obscure names such as Barry Goldwater's vice presidential running mate): "Do you know me? I was the partner of perhaps the best-known futures speculator of our time, Richard Dennis. I was the one who bet Dennis that trading skill could not be taught. The trading group known in the industry as the Turtles was an outgrowth of an experiment to resolve this wager." At this point, the name William Eckhardt might be printed across the screen. [He lost the bet.]

> So who is William Eckhardt? He is a mathematician who just short of earning his Ph.D. took a detour into trading and never returned to academics (at least not officially). Eckhardt spent his early trading years on the floor. Not surprisingly, he eventually abandoned this reflexive trading arena for the more analytical approach of systems-based trading. For a decade, Eckhardt did very well with his own account, primarily based on the signals generated by the systems he developed but supplemented by his own market judgment. During the past five years, Eckhardt has also managed a handful of other accounts, his average return during this period has been 62 percent, ranging from a 7 percent loss in 1989 to a 234 percent gain in 1987. Since 1978, he has averaged better than 60 percent per year in his own trading, with 1989 the only losing year.

> **Were any of the areas you studied in mathematics applicable to developing trading systems?**

> Certainly - statistics. The analysis of commodity markets is prone to pitfalls in classical statistical inference, and if one uses these tools without having a good foundational understanding, it's easy to get into trouble. Most classical applications of statistics are based on the key assumption that the data distribution is normal, or some other known form. Classical statistics work well and allow you to draw precise conclusions if you're correct in your assumption of the data distribution. However, if your distribution assumptions are even a little bit off, the error is enough to derail the delicate statistical estimators, and cruder, robust estimators will yield more accurate results. In general, the delicate tests that statisticians use to squeeze significance out of marginal data have no place in trading. We need blunt statistical instruments, robust techniques.

> **Could you define what you mean by "robust"?**

> A robust statistical estimator is one that is not perturbed much by mistaken assumptions about the nature of the distribution.

> **Why do you feel such techniques are more appropriate for trading system analysis**

> Because I believe that price distributions are pathological.

> **In what way?**

> As one example, price distributions have more variance [a statistical measure of the variability in the data] than one would expect on the basis of normal distribution theory. Benoit Mandelbrot, the originator of the concept of fractional dimension, has conjectured that price change distributions actually have infinite variance. The sample variance [i.e., the implied variability in prices] just gets larger and larger as you add more data. If this is true, then most standard statistical techniques are invalid for price data applications.

> **I don't understand. How can the variance be infinite?**

> A simple example can illustrate how a distribution can have an infinite mean. (By the way, a variance is a mean - it's the mean of the squares of the deviations from another mean.) Consider a simple, one-dimensional random walk generated, say, by the tosses of a fair coin. We are interested in the average waiting time between successive equalizations of heads and tails-that is, the average number of tosses between successive ties in the totals for heads and tails. Typically, if we sample this process, we find that the waiting time between ties tends to be short. This is hardly surprising. Since we always start from a tie situation in measuring the waiting time, another tie is usually not far away. However, sometimes, either heads or tails gets far ahead, albeit rarely, and then we may have to wait an enormous amount of time for another tie, especially since additional tosses are just as likely to increase this discrepancy as to lessen it. Thus, our sample will tend to consist of a lot of relatively short waiting times and a few disquietingly large outliers. What's the average? Remarkably, this distribution has no average, or you can say the average is infinite. At any given stage, your sample average will be finite, of course, but as you gather more sample data, the average will creep up inexorably. If you draw enough sample data, you can make the average in your sample as large as you want.

> **In the coin toss example you just provided, computer simulations make it possible to generate huge data samples that allow you to conclude that the mean has no limit. But how can you definitively state that the variances of commodity price distributions are not finite? Isn't the available data far too limited to draw such a conclusion?**

> There are statistical problems in determining whether the variance of price change is infinite. In some ways, these difficulties are similar to the problems in ascertaining whether we're experiencing global warming. There are suggestive indications that we are, but it is difficult to distinguish the recent rise in temperature from random variation. Getting enough data to assure that price change variance is infinite could take a century.

> **What are the practical implications of the variance not being finite?**

> If the variance is not finite, it means that lurking somewhere out there are more extreme scenarios than you might imagine, certainly more extreme than would be implied by the assumption that prices conform to a normal distribution - an assumption that underlies most statistical applications. We witnessed one example in the one-day 8,000-point drop in the S&P on October 19, 1987. Normal estimation theory would tell you that a one-day price move this large might happen a few times in a millennium. Here we saw it happen within a decade of the inauguration of the S&P contract. This example provides a perfect illustration of the fact that if market prices don't have a finite variance, any classically derived estimate of risk will be significantly understated.



### Breakout Systems

Breakouts are one of the oldest, widest-known trading signals. In other words, they're one of the most common signals novices trade on, and therefore one of the most common signals for professionals to fade. If you wish to trade with a breakout system, you must be willing to be wrong extremely often. Cut your losses short the instant the market tells you the trade isn't working. Perhaps only one or two trades out of ten will show any substantial profits. The benefit of breakout systems is that, when the signal *is* valid, you'll typically have gotten in at the beginning of a trend - and good trends can provide huge profit potential, so long as you let your winners run. You'll hopefully have noticed that if going long upon breakout signals generates losses more often than not, going short on breakouts should do better. And this is precisely what it means to fade a move - just understand that it takes some serious balls to step in front of a charging freight train.


### Trend-Following Systems

These strategies were the first to be algorithmically traded with any kind of serious scale. There are probably three major reasons why this was the case: trend-following is an "obvious" strategy with serious profit potential, trends are "easy" to express programatically with moving averages, and they don't require too much computing power to detect. Perhaps the largest downside of such systems is known as "getting wipsawed." In other words, they do horribly under flat/consolidating/range-constricted conditions. Moving averages are also lagging indicators - by their very definition you'll miss the beginning of the move and get out well after the top - the hope is to catch the meat of the move. The more volatile an instrument is - and the quicker it moves during times of increased volatility - the worse such strategies will do. Trend-following systems remain in widespread use, albeit they are no longer nearly as primitive as simple moving average crossover based systems.


### Oscillator-Based Systems: Range and Momentum Trading

When markets aren't trending, by definition, they are consolidating. The challenge at the far-right edge, obviously, is how to figure out when the switch has occurred. Markets never move in straight lines for very long. Consolidations are essentially chunks of time for the market to breathe after large moves. Sometimes they are continuation patterns in the midst of even longer-term trends, and othertimes, they are reversal periods. These are the places where the smart money sells to the public near/at the top ("distribution"), and buys from the public near/at the bottom ("building a base").

These formations roughly take the shape of rectangles, triangles and flags, and they occur in all time frames (with their significance and predictive power increasing in the longer time frames, of course). Such shapes have [approximately] defined boundaries, of which the price is expected to bounce between (until it finally [typically] breaks out of the formation, beginnning/continueing a new/existing trend). Traders utilize a combination of S/R and stochastic methods to judge their entry and exit points within such trading ranges, hoping to realize a profit by catching the short-term bounces between localized price extremities.

### Classical Arbitrage

After the formalization of exchanges, equities were traded at a central location. There was one best bid and one best ask - per physical exchange "branch" location. But prices diverge between locations, ultimately due to the speed of communication causing information disparities between traders at differing branches. Originally the speed of information flow was limitted by how fast a horse could run, followed by telegraph messages (the ticker tape wasn't introducted at the NYSE until 1867), the phone system, and these days, by the speed of light. Arbitrageurs are the people who wish to capitalize on such price disparities, by simultaneously buying the instrument of interest at the cheaper location and selling it at the higher-priced location. Their role and profit incentive, essentially, is to make markets more efficient. Suffice it to say, these strategies are off limits to all but the largest dealers, brokers and funds who can afford not only to co-locate their servers immediately next to the exchanges' servers around the world, but also to build direct as-the-crow-flies fiber-optic and microwave high-speed communication links between the various exchange and ECN mainframe-warehouses sprinkled across the world. (Microwaves because light travels faster through air than glass, albeit with far lower bandwidth and greater reliability issues thanks primarily to the weather.)


# 21st Century Strategies: Attack of the Quants

> <small>Jack Schwager interviews Jaffray Woodriff, *Hedge Fund Market Wizards*</small>

> Jaffray Woodriff knew three things: He wanted to be a trader; he wanted to use a computerized approach; he wanted to do it differently from anyone else. The majority of futures traders, called CTAs, use trend-following methodologies. These programs seek to identify trends and then take a position in the direction of the trend until a trade liquidation or reversal signal is received. A smaller number of systematic CTAs will use countertrend (also called mean reversion) methodologies. As the name implies, these types of systems will seek to take positions opposite to an ongoing trend when system algorithms signal that the trend is overextended. There is a third category of systematic approaches whose signals do not seek to profit from either continuations or reversals of trend. These types of systems are designed to identify patterns that suggest a greater probability for either higher or lower prices over the near term. Woodriff is among the small minority of CTAs who employ such pattern-recognition approaches, and he does so using his own unique methodology. He is one of the most successful practitioners of systematic trading of any kind.

> **I understand why you would look for a method that was not trend following, given your aversion to following the herd, but why would you inherently avoid a mean reversion approach?**

> For the same reason I didn't pursue trend following - namely, other people were doing the same type of thing. Mean reversion may have been a better fit for me than trend following, but I wanted my own style. I wanted an approach that fit my personality, which is a very important point that I got out of one of your first two Market Wizards books. Mean reversion partially fit my personality, but because people knew about it, it didn't fully fit my personality. So I looked for other ways to crunch the numbers that would be neither trend following nor mean reversion.

> **Without giving away trade secrets, what is the essence of that third approach?**

> I was trying different combinations of secondary variables that I generate from the daily price data.

> **Can you give me an example of what you mean by secondary variables?**

> An example would be a volatility measure, which is a data series that is derived from price, but has no direct relationship to price direction. I got the idea for secondary variables from Bill James.

> **What is the connection between what Bill James did with baseball statistics and what you call secondary variables?**

> James was taking the basic data and formulating different types of statistics that were more informative, and I was taking the price data and defining different quantifications derived from that data, that is, secondary variables, that could be combined to provide useful market signals.

> **Are all your secondary variables derived just from daily open, high, low, and close price data?**

> Absolutely. That is all I am using.

> **You don't throw in any other statistics, such as GNP or any other economic variables?**

> If I could, I would. I actually tried that, but I couldn't get it to work.

> **How would generating these secondary variables give you a trading system?**

> I combined different secondary variables into trend-neutral models.

> **What do you mean by a trend-neutral model?**

> They were neither trying to project a continuation of the trend or a reversal of the trend. They were only trying to predict the probable market direction over the next 24 hours.

> **How many models are there in your system?**

> There are over a thousand.

> **Since there are so many, could you give me an example of just one trend-neutral model to provide a better idea of what you mean? I assume giving away just one out of a thousand models wouldn't reveal a meaningful amount of the system.**

> The issue is that the models share common characteristics. It is hard to give you an example without jeopardizing our intellectual property.

> **Is your system discovery process a matter of seeing patterns in the market and then testing whether they worked, or is it a matter of coming up with theoretical hypotheses and then testing them to see if they worked?**

> I know what to grab.

> [He gets up again in search of another book - this time it is one of mine, Stock Market Wizards. He flips through pages and then finds the spot he is looking for.]

> This is really key. I wouldn't spend the time to do this unless it was really important.

> [Woodriff begins reading my interview with David Shaw. He flips through a few excerpts and then reads the following response by [David] Shaw to my question of how he can tell whether a market pattern represents something real as opposed to a chance occurrence.]

> "The more variables you have, the greater the number of statistical artifacts that you're likely to find, and the more difficult it will generally be to tell whether a pattern you uncover actually has any predictive value. We take great care to avoid methodological pitfalls associated with "overfitting the data." ... Rather than blindly searching through the data for patterns - an approach whose methodological dangers are widely appreciated within, for example, the natural science and medical research communities - we typically start by formulating a hypothesis based on some sort of structural theory or qualitative understanding of the market, and then test that hypothesis to see whether it is supported by the data."

> [Woodriff speaking emphatically] I don't do that . I read all of that just to get to the point that I do what I am not supposed to do, which is a really interesting observation because I am supposed to fail. According to almost everyone, you have to approach systematic trading (and predictive modeling in general) from the framework of "Here is a valid hypothesis that makes sense within the context of the markets." Instead, I blindly search through the data.

> It's nice that people want hypotheses that make sense. But I thought that was very limiting. I want to be able to search the rest of the stuff. I want to automate that process. If you set the problem up really well with cross validation, then overfitting is a problem that can be overcome. I hypothesized that there are patterns that work, and I would rather have the computer test trillions of patterns than just a few hundred that I had thought of.

> There is one aspect of the process, though, that is manual. The secondary variables that are used to construct price-forecasting models have to make sense. For example, it makes sense that price-derived data series, such as volatility or price acceleration, might provide important information. The list of secondary variables derived from price is the part I built manually. Then I have a framework for combining the secondary variables in all sorts of combinations to see what works.

> I wanted to hand that work off to the computer, but I knew how important it was to have the hindsight bias and overfit problem figured out. As an aside, I am still trying to reverse engineer some of the models that we have come up with that are so interesting and amazing. What do these patterns say about the psychology of the marketplace? Frankly, I'm not sure yet.


### Mean Reversion

The name says it all, really. Such strategies make the assumption that the underlying instrument has an average price - and that variations from the mean should revert back to the "right" price, therein showing the trader a profit. In many ways, mean reversion strategies are the quants' take on oscillator-based methods.


### Statistical Arbitrage

> Jack Schwager, *Stock Market Wizards*

> Statistical arbitrage expands the classic arbitrage concept of simultaneously buying and selling identical financial instruments for a locked-in profit to encompass buying and selling closely related financial instruments for a probable profit. In statistical arbitrage, each individual trade is no longer a sure thing, but the odds imply an edge. The trader engaged in statistical arbitrage will lose on a significant percentage of trades but will be profitable over the long run, assuming trade probabilities and transaction costs have been accurately estimated. An appropriate analogy would be roulette (viewed from the casino's perspective): The casino's odds of winning on any particular spin of the wheel are only modestly better than fifty-fifty, but its edge and the laws of probability will assure that it wins over the long run.


### Artificial Intelligence, Machine Learning and Bayesian Methods




# Learning to Trust your Algorithms: Knowing How to Backtest and Analyze Results

First of all, everybody knows the small print: past performance is not necessarily a valid indicator of future performance. Brokerages and funds don't just put this up as some generic disclaimer to cover their asses; it's the stone-cold truth. That said, with enough thorough testing and faith in the notion that market behaviors can and do repeat themselves, we can learn to gain confidence in our systems. The trick, then, is to understand what "thorough testing" means - and how to do just that.

Systems development is full of pitfalls at every turn. Zipline takes care of the basics for you - accounting for commission and slippage transaction costs while protecting you from look-ahead bias in your algorithms. These are extremely important considerations, to be sure, but they alone cannot save you from yourself.

If you'll permit me to take a brief journey back to middle-school science class, you'll probably remember the notion of the null hypothesis. Simply stated, one operates under the assumption that there is zero relationship between N variables, and it is only after we have *statistically significant* evidence strongly suggesting otherwise that we can reasonably begin to consider the possibility that there may be causation in the mix of our variables. (Because, obviously, correlation alone is not an indicator of causation.)

David Shaw had this to say on the subject:

> **When you find an apparent anomaly or pattern in the historical data, how do you know it represents something real as opposed to a chance occurrence?**

> The more variables you have, the greater the number of statistical artifacts that you're likely to find, and the more difficult it will generally be to tell whether a pattern you uncover actually has any predictive value. We take great care to avoid the methodological pitfalls associated with "overfitting the data."

> Although we use a number of different mathematical techniques to establish the robustness and predictive value of our strategies, one of our most powerful tools is the straightforward application of the scientific method. Rather than blindly searching through the data for patterns - an approach whose methodological dangers are widely appreciated within, for example, the natural science and medical research communities - we typically start by formulating a hypothesis based on some sort of structural theory or qualitative understanding of the market, and then test that hypothesis to see whether it is supported by the data.

> Unfortunately, the most common outcome is that the actual data fail to provide evidence that would allow us to reject the "null hypothesis" of market efficiency. Every once in a while, though, we do find a new market anomaly that passes all our tests, and which we wind up incorporating in an actual trading strategy.

When drug companies test their newest concoctions on subjects, they (hopefully) start with simulations, followed by animal test subjects. They test, observe, improve and optimize - and repeat. Sooner or later, they've gotten enough evidence to suggest their concoction is probably safe to test on humans. And, if the concoction tests well enough on a large enough set of humans against a control group - and the FDA gives them the goahead - the company gets to give their concoction a name and sell it to you for absurd amounts of money.

Our process is not so different. There's no FDA to worry about needing approval from - yet? - but replace test-subjects with historical data samples and you've got a good description of how algorithmic systems [should] get developed. We develop an elementary version of our system on a set of data - approximately a third of the data available to us; the **in-sample** - and proceed to optimize it on a *different* third of data. Finally, we test it on the last third of data never seen before by the system - the **out-sample** - and if the system *still* performs acceptably, there's a half-decent chance the system might survive in the open marketplace. Clean, never seen before data is the closest you can get to live testing without risking real capital. It's rumored that some HFT outfits simply do away with all this upfront work and simply turn their systems on in the live marketplace to do their testing - it hopefully goes without saying that such testing strategies make the definition of foolish look silly.

Why so many steps, you may ask? The answer primarily comes down to protecting yourself from [inadvertant] curve-fitting and over-optimization. (Optimization refers to the process of testing many variations of a system for the past and then selecting the best-performing version for actual trading.) Given a fixed set of in-sample data, it's rather easy to develop a system that looks capable of producing incredible profits. Such algorithms are a-dime-a-dozen on Quantopian.com. Everything looks great on paper - that is, until you try the algorithm in differing market conditions from those under which it was developed. Set-it-and-forget-it systems must be capable of operating successfully accross all markets, in uptrends, downtrends, consolidation ranges, flat markets, volatile markets and choppy markets, not to mention any other random noise you *will* encounter in the financial markets.

> William Eckhardt:

> **What is your opinion about optimization?**

> It's a valid part of the mechanical trader's repertoire, but if you don't use methodological care in optimization, you'll get results that are not reproducible.

> **How do you avoid that pitfall?**

> You really are caught between conflicting objectives. If you avoid optimization altogether, you're going to end up with a system that is vastly inferior to what it could be. If you optimize too much, however, you'll end up with a system that tells you more about the past than the future. Somehow, you have to mediate between these two extremes.

> **[What other] advice do you have for people who are involved in system development?**

> If the performance results of the system don't sock you in the eye, then it's probably not worth pursuing. It has to be an outstanding result. Also, if you need delicate, assumption-laden statistical techniques to get superior performance results, then you should be very suspicious of the system's validity.

> **As a general rule, be very skeptical of your results. The better a system looks, the more adamant you should be in trying to disprove it.** This idea goes very much against human nature, which wants to make the historical performance of a system look as good as possible.

> Karl Popper has championed the idea that all progress in knowledge results from efforts to falsify, not to confirm, our theories. Whether or not this hypothesis is true in general, it's certainly the right attitude to bring to trading research. You have to try your best to disprove your results. You have to try to kill your little creation. Try to think of everything that could be wrong with your system, and everything that's suspicious about it. If you challenge your system by sincerely trying to disprove it, then maybe, just maybe, it's valid.


> Jaffray Woodriff:

> **When you first started managing money under Blue Ridge, for the three months in your inception year and the subsequent two full years, you were actually down slightly on balance. Then in the first six months of the following year, you were up over 80 percent. That is such a stark contrast in performance that it seems that something significant must have changed in your trading approach during those early years. Was there some major change to your methodology during this period, and if so what was it?**

> I started out using market-specific models. I ended up realizing that these models were far more vulnerable to breaking down in actual trading because they were more prone to being overfitted to the past data. In 1993, I started to figure out that the more data I used to train the models, the better the performance. I found that using the same models across multiple markets provided a far more robust approach. So the big change that occurred during this period was moving from separate models for each market to common models applied across all markets.

> The second change that occurred was increased diversification. I started out trading only two markets and then for some time traded only three markets. But as assets under management increased, and I realized it was best to use the same models across all markets, I added substantially more markets to the portfolio. The transition to greater diversification also helped improve performance. By 1994, I was trading about 20 markets, and I was no longer using market-specific models. Those changes made a big difference.

Obviously, one needs *a lot* of sample data to do this correctly. How much exactly depends on many factors but the ultimate goal is for the system to produce enough trading signals that you've gotten a statistically significant result set from which to base your conclusions on. The minimum number of trade signals to accomplish this varies widely depending on who you ask and the type of system you're developing but suffice it to say, the number of signals should be well into the triple digits.

Even more important than simply having a large result set is the quality of the result set itself. Aggregating these results into a single number - the total return of the system - and basing your conclusions upon that number alone is a sure-fire way to walk into the markets with a gun to your head. You need to take a much closer look at the distribution of your systems' results. What is the ratio of winners to losers? How large are the average (mean *and* median) profits and losses? How extreme are the single-trade outliers - does the system show a profit only because of two or three lucky winners? What are the longest winning and, more importantly, losing streaks? How does that compare with the largest drawdown experienced by the system? Could your trading account handle such an assault right out of the gates, and is that something you'd be comfortable with? 

> <small>Marcel Link, *High Probability Trading*</small>

> I once spent months writing a system with which to day trade the S&Ps. I backtested and edited until it was, I thought, the perfect system. Its only drawback was that it had a high number of consecutive losing trades, but I didn't think I had to worry about that at the beginning. The winning trades were much bigger than the losers, and so the system was very profitable. I was confident it would make money from the start so that when the losers came, it wouldn't hurt.

> As you probably have guessed, the losing streak started immediately: the first eight trades were losers, and they put me and my partner about $12,000 in the hole, which we were not prepared for. We had to abandon the system and slow down our trading for a while, and as luck would have it, the very next trade was a huge winner. Over the next few trades the system would have made back all its losses and then some. The moral here is to know the system's drawdown and make sure you can afford it.


# Money Management and Risk Control










