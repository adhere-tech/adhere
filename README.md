# AdHere

AdHere is an automated framework that assesses compliance with the Better Ads Standards and
helps website developers to detect and fix the violations.
AdHere can pinpoint ads on the fly and identify participating ad networks. 
It can also provide fix suggestions for developers and 
assist developers in improving their fix solutions while preserving more ad revenue.

This repository contains the source code of AdHere, 
the script used for the preliminary study, and the dataset.
The Preliminary Study Toolset is used to get the compliance status from Google Ad Experience Report.

## Dependencies
- Python 3 (recommended >= 3.7)  
- Python 3 libraries: Selenium, lxml, wmi, pywin32, requests
- Google Chrome (recommended version >= 79) 
- [ChromeDriver](https://chromedriver.chromium.org/): Version corresponding to Chrome version and OS

## Setup Instructions

Please first download 
the source code [here](https://github.com/adhere-tech/adhere-tech.github.io/tree/master/SourceCode).
#### AdHere
1. Install Python 3 and all dependencies.
2. Based on the OS and Chrome version, 
   download the corresponding version of Chromedriver [here](https://chromedriver.chromium.org/). 
   Unzip the downloaded file and put `chromedriver.exe` in the same folder as `AdHere.py`.
3. Fill Google Chrome's user profile directory after `USRPROFILE = ` in `AdHere.py`.
4. Run `python AdHere.py domain_url` in the command line to run AdHere on the given URL. 
   Leaving `domain_url` blank will perform a self-inspection on google.com. 
   It will scan the website with the headless (no GUI) Google Chrome. After finishing the scan, 
   AdHere will generate `violations.txt` in the same folder as `AdHere.py`. 
   The text file contains violations (i.e., the id, violation type, and XPath) and their fix suggestions.
    
#### Preliminary Study Toolset
1. Install Python 3 and all [dependencies](adhere-tech.github.io#dependencies).
2. Create at least one project using Google Ad Experience Report API in Google Developer Console. 
   Apply for the API key for each project.
3. Fill API key(s) in `API_KEY_LIST` in `google.py`. 
   Adjust `THREAD_COUNT` based on the comments in `google.py`.
4. Run `python google.py` in the terminal to get Google Ad Experience Report's result on the Alexa Top 1 Million Websites. 
   Try to keep the network connection stable.
5. In the generated files,`[R]Alexa_done [XX_XX].csv` is the raw file to be stored in the database. 
   It records the compliance status of the 1 million websites.


## Dataset

Due to the large volume of data, this section only lists 
a small portion of the data generated from our preliminary study and the evaluation of AdHere.
Complete dataset (1.1 GB tar.gz file) can be found [here (Google Drive)](https://drive.google.com/file/d/1lUGUj2bLEhMzKzJ-5goCx7bAqc7tN3Zc/view?usp=sharing)
or [here (Zenodo)](https://doi.org/10.5281/zenodo.5700434).

#### Alexa-result_Sep_10_2020.csv
This file contains Google Ad Experience Report’s result samples
of the Alexa Top 1 Million Websites collected on September 10th, 2020.
<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSGx9nPZIDXRfVwhFt4rTM_bVbWagirlg6wVuvP2bB79_0ZndvWe_ZTi_BVyWEacVhPUchVuTF4R2yr/pubhtml?gid=2041000367&amp;single=true&amp;widget=true&amp;headers=false" loading allowfullscreen width="100%" height="300"></iframe>

#### fix_list.csv
This file contains the raw data of violating ad fix time.
<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSGx9nPZIDXRfVwhFt4rTM_bVbWagirlg6wVuvP2bB79_0ZndvWe_ZTi_BVyWEacVhPUchVuTF4R2yr/pubhtml?gid=1068233299&amp;single=true&amp;widget=true&amp;headers=false" loading allowfullscreen width="100%" height="300"></iframe>

#### adhere_partial.csv
This file contains violating ad samples found by AdHere.
<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSGx9nPZIDXRfVwhFt4rTM_bVbWagirlg6wVuvP2bB79_0ZndvWe_ZTi_BVyWEacVhPUchVuTF4R2yr/pubhtml?gid=911669432&amp;single=true&amp;widget=true&amp;headers=false" loading allowfullscreen width="100%" height="300"></iframe>

#### top_1m.csv
This is the samples of Alexa Top 1 Million Websites list.
<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSGx9nPZIDXRfVwhFt4rTM_bVbWagirlg6wVuvP2bB79_0ZndvWe_ZTi_BVyWEacVhPUchVuTF4R2yr/pubhtml?gid=2070170508&amp;single=true&amp;widget=true&amp;headers=false" loading allowfullscreen width="100%" height="300"></iframe>

#### fix_example
This file contains the source code of the fix example on "getsongbpm.com". 
Details about this example can be found [in Finding 3](#finding-3-fix-with-attribute-modification---a-case-study).

#### manual_inspection.xlsx
This file contains the manual inspection result on 1,000 websites. It shows the performance of AdHere and Google
for each combination of platform and Google status.
The sections with orange text shows the per-category data. 
The sections with black text shows the per-website data.
<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSGx9nPZIDXRfVwhFt4rTM_bVbWagirlg6wVuvP2bB79_0ZndvWe_ZTi_BVyWEacVhPUchVuTF4R2yr/pubhtml?gid=1562090962&amp;single=true&amp;widget=true&amp;headers=false" loading allowfullscreen width="100%" height="300"></iframe>

## Findings

#### Finding 1. Website Coverage
![Unable to display figure1. Check browser settings.](figs/data_8.png)

Among the websites being reviewed by Google, the average
number of mobile websites failed daily is 884 while the desktop
version is 690. Moreover, the average numbers of websites passed
daily are 53,353 for mobile and 61,025 for desktop. The above figure shows the
number of sites with the PASSING status and FAILING status from
April 13, 2019 to August 18, 2019.
In general, among those that have been reviewed, we observed a
consistent trend that mobile websites contain more violating
ads than the desktop version. However, the numbers of violating
sites for both platforms are declining.

#### Finding 2. Ad Networks
![Unable to display table1. Check browser settings.](figs/table_4.png)

The above table shows ad network statistics from ads collected
from AdHere-detected violating websites (on August 19th, 2020).
As can be seen, ExoClick, Google Ads, and JUICYADS were the
most prominent ad networks presented the tendency to deliver violating
ads. Note that these top networks delivering violating
ads are not necessarily popular networks. According to the statistics
provided by W3Techs, 71.4% (15 out of 21) of these networks
are unranked and have market share much less than 0.1%.

#### Finding 3. Fix with Attribute Modification - A Case Study

The fix example happens on the mobile website "getsongbpm.com", 
a website telling the bpm (beats per minute) of songs. 
The following figure shows how a violating ad was fixed.
The screenshot on the left shows the web page before the fix 
and the screenshot on the right shows the page after the fix. 
The red square on the left highlights a Pop-up Ad that blocks interactions with other elements. 
Based on the Better Ads Standards, this Pop-up Ad is violating the standards. 
A non-violating ad was used to fix the violation. 
The green square on the right shows that the Pop-up was replaced by a non-violating ad. 

![Unable to display figure2. Check browser settings.](figs/merged.png)

The code snippets of the page before and after the fix are shown below.

Code snippet of Pop-up Ad (line numbers are positions in the source file):
```html
   1    <html><body>...
5694    <!--ADHERE: pop-up ad starts. You can find this comment at line 5694 in file-->
5695    <div id="aic-root-container-250">...
5697        <iframe id="aic-frame-66">...
5700            <html><body><div><div>...
5930                <div id="ad_unit">
5931                    <div class="GoogleActiveViewElement">...
5938                        <div class="GoogleCreativeContainerClass">
                                <!--ADHERE: ad image. You can find this comment at line 5938 in file-->
5938                            <a target="_blank" src="https://s0.2mdn.net/simgad/..." alt="Advertisement"></a>
                            </div>
                        </div>
                    </div>
                </div></div></body></html>
            </iframe>
        </div>...
6322    <!--ADHERE: pop-up ad ends. You can find this comment at line 6322 in file-->
        </body></html>
```

Code snippet of the non-violating ad:
```html
   1    <html><body>...
4712    <section id="content">...
4752        <div class="container"><div>...
4754            <!--ADHERE: normal ad starts. You can find this comment at line 4754 in file-->
4755            <div class="leadboard addmt"><div>...
4772                <div id="google_ads_iframe_/53015287/getsongbpm.com_m_300x250_1_0__container__">...
4774                    <iframe id="google_ads_iframe_/53015287/getsongbpm.com_m_300x250_1_0">...
4780                        <html><body><div>...
5587                            <div id="google_image_div">...
5588                                <a href="https://www.googleadservices.com/pagead/..." target="_blank">
5590                                    <amp-img>...
                                            <!--ADHERE: ad image hosted on Google Ads. You can find this comment at line 5594 in file-->
5594                                        <img src="https://tpc.googlesyndication.com/daca_images/simgad/...">
                                        </amp-img>
                                    </a>
                                </div>
                            </div></body></html>
                        </iframe>
                    </div>
                </div></div>...
6035            <!--ADHERE: normal ad ends. You can find this comment at line 6035 in file-->
            </div></div>
        </section></body></html>
```
