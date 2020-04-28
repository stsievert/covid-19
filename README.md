Data sources include the following:

* Infections per country: [Worldometers]
* <span style="text-decoration: line-through;">Infections in the US: the [John
  Hopkins COVID-19 dataset][jh-covid19].</span>
* Infections in the US: the [New York Times US COVID-19
  dataset][nytimesdataset]
* The number of hospitals beds: Wikipedia page on [List of OECD countries by
  hospital beds]
* The population of each country: Wikipedia pages on each country (e.g.,
  [United States])
* "[The coronavirus numbers we should really be worried about][wapost-numbers]"
  by the Washington Post to estimate the number of ventilators and hospital
  beds
* "[U.S. Resource Availability for COVID-19]" to estimate the number of acute
  care beds and ICU beds.
* [This twitter thread by @LizSpecht] to estimate the hospitalization rate
  (yes, it's a *rough* approximation).
* The Metropolitan Transportation Authority's (MTA) [public turnstile
  data][mta-subway]. I have used the parsing by [Todd Schneider] at [New York
  City Subway Usage Dashboard][nyc-subway], available through the JSON API he
  developed.
* NYC's Dept. of Health's data on coronavirus, available at [NYC DOH:
  COVID-19][nyc-doh] and at their Github repo
  [nychealth/coronavirus-data][nyc-gh]

[nyc-doh]:https://www1.nyc.gov/site/doh/covid/covid-19-data.page
[nyc-gh]:https://github.com/nychealth/coronavirus-data

COVID-19 kills older people more often. 99% of the deaths are for people that
are 50 or older. That's not to say younger people who contract the virus does
not have impacts: "some recovered patients may have reduced lung function and
are left gasping for air while walking briskly" ([source][scmp]).


[mta-subway]:http://web.mta.info/developers/turnstile.html
[scmp]:https://www.scmp.com/news/hong-kong/health-environment/article/3074988/coronavirus-some-recovered-patients-may-have

Here's the fraction of each countries population that is over 65:

* South Korea: 10.7% in 2010 ([source](https://en.wikipedia.org/wiki/Demographics_of_South_Korea#Aging_population))
* US: 16.03% in ? ([source](https://en.wikipedia.org/wiki/Demographics_of_the_United_States#Structure))
* Italy: 21.69% in 2018 ([source](https://en.wikipedia.org/wiki/Demographics_of_Italy#Demographic_statistics))
* Germany: 22.36% ([source](https://en.wikipedia.org/wiki/Demographics_of_Germany#Demographic_statistics))
* UK: 18.0% in 2016
  ([source](https://en.wikipedia.org/wiki/Demography_of_the_United_Kingdom#Age_structure))

Here are the number on critical care beds per capita
([source][stat-ccb]):

[stat-ccb]:https://www.statista.com/chart/21105/number-of-critical-care-beds-per-100000-inhabitants/

<img src="static-imgs/critical-care-beds.jpg" width="60%"
class="center" />

The Wikipedia page on "[2020 coronavirus pandemic in the United States]" also
has this graph.

"Health care saturation" dates:

* March 11th for Italy because of these pieces: [Bloomberg on March 10th],
  [NYTimes on March 12th].

Social distancing dates:

* Italy: Feb. 25th, chosen because on that day [the following happening in
  Italy][italy-sd]:
    * Morgan Stanley, Barclays, Mediobanca and UniCredit requested their
      employees work from home.
    * The University of Palermo suspended all activities.
    * The Italian Basketball Federation suspended all of its championship
      games.
    * The Italian stock index (FTSE MID) fell by 6%.
    * (Feb. 23rd) Last two days of the Carnival of Venice are cancelled
    * (Feb. 24th) The Basilicata instituted a 14-day mandatory quarantine for people arriving from Northern Italy
* US: March 12th because [the following happened in the US][us-sd]:
    * On March 11th or 12th, many universities announced canceled class would
      be canceled or held online when the students returned from Spring Break
      (including Cornell, Harvard, USC, UMN and UW–Madison).
    * On March 11th, Google, Facebook, Microsoft and E3 announced their
      technology conferences would be held online. Apple announced their
      technology conference would be moved online on March 13th.
    * On March 12th, The NHL, MLS and Women's Soccer League announced they
      would cancel their season.
    * On March 12th, the NCAA announced it would cancel March Madness. 2020 is
      the first year in it's 81-year history that it will not be held.
* UK: March 19th because on March 18th, 19th and 20th [the following happened
  in the UK][uk-sd]:
    * On March 18th, McDonald's and others "announced that they would not
      permit customers to sit and eat in stores"
    * On March 18th, many schools announced all university buildings would be
      closed to students and staff on March 20th (that Friday), including
      Welsh, Scottish and Northern Ireland K-12 schools and Cambridge
      University ([detail][tg-schools]).
    * On March 19th, a Sheffield light rail network and Yorkshire bus operators
      announced a reduced timetable


For the restaurant bookings chart, I used the data from OpenTable's "[The state of the restaurant industry]"

### Health care breakdown estimation
The number of infections I estimate each country can handle comes from Italy's
health care system being overrun, which happened when the had approximately
12,500 cases when their health care system reached capacity. I scale each
countries infection density by the percent of the population over 65 and the
number of "critical care beds".  This is a back-of-the-envelope calculation
that only relies on all health care systems being equally capable.

To estimate the number of cases each country can handle, I use this
equation:

``` python
# Measures how much greater concentration the given country can handle
ratio = (frac_over_65 / italy_frac_over_65) * (n_beds / italy_beds)

# Scale by population
limit = ratio * (pop / italy_pop) * italy_cases_breakdown
```

[tg-schools]:https://www.theguardian.com/world/2020/mar/18/coronavirus-uk-schools-to-be-closed-indefinitely-and-exams-cancelled
[us-sd]:https://en.wikipedia.org/wiki/2020_coronavirus_pandemic_in_the_United_States#Other_reactions

[uk-sd]:https://en.wikipedia.org/wiki/2020_coronavirus_pandemic_in_the_United_Kingdom#Food_and_hospitality
[italy-sd]:https://en.wikipedia.org/wiki/2020_coronavirus_pandemic_in_Italy#Management
[nytimesdataset]:https://github.com/nytimes/covid-19-data


[jh-covid19]:https://github.com/CSSEGISandData/COVID-19

[The state of the restaurant industry]:https://www.opentable.com/state-of-industry
[wapost-numbers]:https://www.washingtonpost.com/health/2020/03/13/coronavirus-numbers-we-really-should-be-worried-about/
[U.S. Resource Availability for COVID-19]:https://www.sccm.org/getattachment/Blog/March-2020/United-States-Resource-Availability-for-COVID-19/United-States-Resource-Availability-for-COVID-19.pdf
[This twitter thread by @LizSpecht]:https://twitter.com/LizSpecht/status/1236095180459003909

[2020 coronavirus pandemic in the United States]:https://en.wikipedia.org/wiki/2020_coronavirus_pandemic_in_the_United_States


[United States]:https://en.wikipedia.org/wiki/United_States
[Worldometers]:https://www.worldometers.info/coronavirus/
[List of OECD countries by hospital beds]:https://en.wikipedia.org/wiki/List_of_OECD_countries_by_hospital_beds
[who-pandemic]:https://www.who.int/dg/speeches/detail/who-director-general-s-opening-remarks-at-the-media-briefing-on-covid-19---11-march-2020
[us-ntl-emergency]:https://en.wikipedia.org/wiki/List_of_national_emergencies_in_the_United_States
[nyc-subway]:https://toddwschneider.com/dashboards/nyc-subway-turnstiles/

[NYTimes on March 12th]:https://www.nytimes.com/2020/03/12/world/europe/12italy-coronavirus-health-care.html
[Bloomberg on March 10th]:https://www.bloomberg.com/news/articles/2020-03-10/virus-spread-pushes-italian-hospitals-toward-breaking-point
[Todd Schneider]:https://toddwschneider.com
