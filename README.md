

```python
import pandas as pd
import numpy as np
import scipy.stats as st
import xlrd 
import matplotlib.pyplot as plt
import datetime
import seaborn.apionly as sns
%matplotlib inline


data =pd.read_csv("data.csv") 
data= data.drop_duplicates(subset=['Full Name','Year','Laureate ID'])
data=data.replace({'Death Date':"alive"},value="11/26/2019",regex=True)
data=data.replace({'Death Date':"not found"},value="11/26/2019",regex=True)
data = data[data["Laureate Type"] != 'Organization']
data=data.reset_index(drop=True)

data['Birth Year']= data["Birth Date"].str.extract("(\d{4})", expand=False)  
data['Death Year']= data["Death Date"].str.extract("(\d{4})", expand=False)  
data["Age"]=-data['Birth Year'].astype('int')+data["Death Year"].astype('int')
data["Age_Win"]=-data['Birth Year'].astype('int')+data["Year"].astype('int')
data.drop(['Death City', 'Death Country',"Birth Date","Death Date","Birth Year","Death Year","Motivation","Prize","Laureate Type"], axis = 1,inplace = True)
data.drop(['Unnamed: 18'], axis = 1,inplace = True)
data['Birth Country']= data['Birth Country'].str.replace(r'[^(]*\(|\)[^)]*', '')

d=data.groupby('Birth Country')["Organization Country"].apply(lambda x: st.mode(x)[0][0])
d=d[data['Birth Country']]
d_null=data['Organization Country'].isnull()

for i in range(0,data['Birth Country'].size):
    
    if(d_null[i]):
        # print("s")
        if(d[i]==0):
            data.iat[i,10] =data['Birth Country'][i] 
        else:
            data.iat[i,10] =d[i]

data.to_csv("new_data.csv")
data

# t=data.isnull().sum()
# t
```

    c:\users\jack of all trade\appdata\local\programs\python\python36\lib\_collections_abc.py:841: MatplotlibDeprecationWarning: 
    The examples.directory rcparam was deprecated in Matplotlib 3.0 and will be removed in 3.2. In the future, examples will be found relative to the 'datapath' directory.
      self[key] = other[key]
    c:\users\jack of all trade\appdata\local\programs\python\python36\lib\_collections_abc.py:841: MatplotlibDeprecationWarning: 
    The savefig.frameon rcparam was deprecated in Matplotlib 3.1 and will be removed in 3.3.
      self[key] = other[key]
    c:\users\jack of all trade\appdata\local\programs\python\python36\lib\_collections_abc.py:841: MatplotlibDeprecationWarning: 
    The text.latex.unicode rcparam was deprecated in Matplotlib 3.0 and will be removed in 3.2.
      self[key] = other[key]
    c:\users\jack of all trade\appdata\local\programs\python\python36\lib\_collections_abc.py:841: MatplotlibDeprecationWarning: 
    The verbose.fileo rcparam was deprecated in Matplotlib 3.1 and will be removed in 3.3.
      self[key] = other[key]
    c:\users\jack of all trade\appdata\local\programs\python\python36\lib\_collections_abc.py:841: MatplotlibDeprecationWarning: 
    The verbose.level rcparam was deprecated in Matplotlib 3.1 and will be removed in 3.3.
      self[key] = other[key]
    c:\users\jack of all trade\appdata\local\programs\python\python36\lib\site-packages\seaborn\apionly.py:9: UserWarning: As seaborn no longer sets a default style on import, the seaborn.apionly module is deprecated. It will be removed in a future version.
      warnings.warn(msg, UserWarning)
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Full Name</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Age</th>
      <th>Age_Win</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1901</td>
      <td>Chemistry</td>
      <td>1-Jan</td>
      <td>160</td>
      <td>Jacobus Henricus van 't Hoff</td>
      <td>Rotterdam</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>Berlin University</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>59</td>
      <td>49</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1901</td>
      <td>Literature</td>
      <td>1-Jan</td>
      <td>569</td>
      <td>Sully Prudhomme</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>France</td>
      <td>68</td>
      <td>62</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1901</td>
      <td>Medicine</td>
      <td>1-Jan</td>
      <td>293</td>
      <td>Emil Adolf von Behring</td>
      <td>Hansdorf (Lawice)</td>
      <td>Poland</td>
      <td>Male</td>
      <td>Marburg University</td>
      <td>Marburg</td>
      <td>Germany</td>
      <td>63</td>
      <td>47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1901</td>
      <td>Peace</td>
      <td>2-Jan</td>
      <td>462</td>
      <td>Jean Henry Dunant</td>
      <td>Geneva</td>
      <td>Switzerland</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Switzerland</td>
      <td>82</td>
      <td>73</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1901</td>
      <td>Peace</td>
      <td>2-Jan</td>
      <td>463</td>
      <td>Frédéric Passy</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>France</td>
      <td>90</td>
      <td>79</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1901</td>
      <td>Physics</td>
      <td>1-Jan</td>
      <td>1</td>
      <td>Wilhelm Conrad Röntgen</td>
      <td>Lennep (Remscheid)</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Munich University</td>
      <td>Munich</td>
      <td>Germany</td>
      <td>78</td>
      <td>56</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1902</td>
      <td>Chemistry</td>
      <td>1-Jan</td>
      <td>161</td>
      <td>Hermann Emil Fischer</td>
      <td>Euskirchen</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Berlin University</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>67</td>
      <td>50</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1902</td>
      <td>Literature</td>
      <td>1-Jan</td>
      <td>571</td>
      <td>Christian Matthias Theodor Mommsen</td>
      <td>Garding</td>
      <td>Germany</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Germany</td>
      <td>86</td>
      <td>85</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1902</td>
      <td>Medicine</td>
      <td>1-Jan</td>
      <td>294</td>
      <td>Ronald Ross</td>
      <td>Almora</td>
      <td>India</td>
      <td>Male</td>
      <td>University College</td>
      <td>Liverpool</td>
      <td>United Kingdom</td>
      <td>75</td>
      <td>45</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1902</td>
      <td>Peace</td>
      <td>2-Jan</td>
      <td>464</td>
      <td>Élie Ducommun</td>
      <td>Geneva</td>
      <td>Switzerland</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Switzerland</td>
      <td>73</td>
      <td>69</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1902</td>
      <td>Peace</td>
      <td>2-Jan</td>
      <td>465</td>
      <td>Charles Albert Gobat</td>
      <td>Tramelan</td>
      <td>Switzerland</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Switzerland</td>
      <td>71</td>
      <td>59</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1902</td>
      <td>Physics</td>
      <td>2-Jan</td>
      <td>2</td>
      <td>Hendrik Antoon Lorentz</td>
      <td>Arnhem</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>Leiden University</td>
      <td>Leiden</td>
      <td>Netherlands</td>
      <td>75</td>
      <td>49</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1902</td>
      <td>Physics</td>
      <td>2-Jan</td>
      <td>3</td>
      <td>Pieter Zeeman</td>
      <td>Zonnemaire</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>Amsterdam University</td>
      <td>Amsterdam</td>
      <td>Netherlands</td>
      <td>78</td>
      <td>37</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1903</td>
      <td>Chemistry</td>
      <td>1-Jan</td>
      <td>162</td>
      <td>Svante August Arrhenius</td>
      <td>Vik</td>
      <td>Sweden</td>
      <td>Male</td>
      <td>Stockholm University</td>
      <td>Stockholm</td>
      <td>Sweden</td>
      <td>68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1903</td>
      <td>Literature</td>
      <td>1-Jan</td>
      <td>572</td>
      <td>Bjørnstjerne Martinus Bjørnson</td>
      <td>Kvikne</td>
      <td>Norway</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Norway</td>
      <td>78</td>
      <td>71</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1903</td>
      <td>Medicine</td>
      <td>1-Jan</td>
      <td>295</td>
      <td>Niels Ryberg Finsen</td>
      <td>Thorshavn</td>
      <td>Denmark</td>
      <td>Male</td>
      <td>Finsen Medical Light Institute</td>
      <td>Copenhagen</td>
      <td>Denmark</td>
      <td>44</td>
      <td>43</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1903</td>
      <td>Peace</td>
      <td>1-Jan</td>
      <td>466</td>
      <td>William Randal Cremer</td>
      <td>Fareham</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United Kingdom</td>
      <td>80</td>
      <td>75</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1903</td>
      <td>Physics</td>
      <td>2-Jan</td>
      <td>4</td>
      <td>Antoine Henri Becquerel</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>École Polytechnique</td>
      <td>Paris</td>
      <td>France</td>
      <td>56</td>
      <td>51</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1903</td>
      <td>Physics</td>
      <td>4-Jan</td>
      <td>5</td>
      <td>Pierre Curie</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>École municipale de physique et de chimie indu...</td>
      <td>Paris</td>
      <td>France</td>
      <td>47</td>
      <td>44</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1903</td>
      <td>Physics</td>
      <td>4-Jan</td>
      <td>6</td>
      <td>Marie Curie, née Sklodowska</td>
      <td>Warsaw</td>
      <td>Poland</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United States of America</td>
      <td>67</td>
      <td>36</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1904</td>
      <td>Chemistry</td>
      <td>1-Jan</td>
      <td>163</td>
      <td>Sir William Ramsay</td>
      <td>Glasgow</td>
      <td>Scotland</td>
      <td>Male</td>
      <td>University College</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>64</td>
      <td>52</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1904</td>
      <td>Literature</td>
      <td>2-Jan</td>
      <td>573</td>
      <td>Frédéric Mistral</td>
      <td>Maillane</td>
      <td>France</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>France</td>
      <td>84</td>
      <td>74</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1904</td>
      <td>Literature</td>
      <td>2-Jan</td>
      <td>574</td>
      <td>José Echegaray y Eizaguirre</td>
      <td>Madrid</td>
      <td>Spain</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Spain</td>
      <td>84</td>
      <td>72</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1904</td>
      <td>Medicine</td>
      <td>1-Jan</td>
      <td>296</td>
      <td>Ivan Petrovich Pavlov</td>
      <td>Ryazan</td>
      <td>Russia</td>
      <td>Male</td>
      <td>Military Medical Academy</td>
      <td>St. Petersburg</td>
      <td>Russia</td>
      <td>87</td>
      <td>55</td>
    </tr>
    <tr>
      <th>24</th>
      <td>1904</td>
      <td>Physics</td>
      <td>1-Jan</td>
      <td>8</td>
      <td>Lord Rayleigh (John William Strutt)</td>
      <td>Langford Grove, Maldon, Essex</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Royal Institution of Great Britain</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>77</td>
      <td>62</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1905</td>
      <td>Chemistry</td>
      <td>1-Jan</td>
      <td>164</td>
      <td>Johann Friedrich Wilhelm Adolf von Baeyer</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Munich University</td>
      <td>Munich</td>
      <td>Germany</td>
      <td>82</td>
      <td>70</td>
    </tr>
    <tr>
      <th>26</th>
      <td>1905</td>
      <td>Literature</td>
      <td>1-Jan</td>
      <td>575</td>
      <td>Henryk Sienkiewicz</td>
      <td>Wola Okrzejska</td>
      <td>Poland</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United States of America</td>
      <td>70</td>
      <td>59</td>
    </tr>
    <tr>
      <th>27</th>
      <td>1905</td>
      <td>Medicine</td>
      <td>1-Jan</td>
      <td>297</td>
      <td>Robert Koch</td>
      <td>Clausthal (Clausthal-Zellerfeld)</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Institute for Infectious Diseases</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>67</td>
      <td>62</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1905</td>
      <td>Peace</td>
      <td>1-Jan</td>
      <td>468</td>
      <td>Baroness Bertha Sophie Felicita von Suttner, n...</td>
      <td>Prague</td>
      <td>Czech Republic</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United States of America</td>
      <td>71</td>
      <td>62</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1905</td>
      <td>Physics</td>
      <td>1-Jan</td>
      <td>9</td>
      <td>Philipp Eduard Anton von Lenard</td>
      <td>Pressburg (Bratislava)</td>
      <td>Slovakia</td>
      <td>Male</td>
      <td>Kiel University</td>
      <td>Kiel</td>
      <td>Germany</td>
      <td>85</td>
      <td>43</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>852</th>
      <td>2014</td>
      <td>Literature</td>
      <td>1-Jan</td>
      <td>912</td>
      <td>Patrick Modiano</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>France</td>
      <td>74</td>
      <td>69</td>
    </tr>
    <tr>
      <th>853</th>
      <td>2014</td>
      <td>Medicine</td>
      <td>2-Jan</td>
      <td>903</td>
      <td>John O'Keefe</td>
      <td>New York, NY</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University College</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>80</td>
      <td>75</td>
    </tr>
    <tr>
      <th>854</th>
      <td>2014</td>
      <td>Medicine</td>
      <td>4-Jan</td>
      <td>904</td>
      <td>May-Britt Moser</td>
      <td>FosnavÃ¥g</td>
      <td>Norway</td>
      <td>Female</td>
      <td>Norwegian University of Science and Technology...</td>
      <td>Trondheim</td>
      <td>Norway</td>
      <td>56</td>
      <td>51</td>
    </tr>
    <tr>
      <th>855</th>
      <td>2014</td>
      <td>Medicine</td>
      <td>4-Jan</td>
      <td>905</td>
      <td>Edvard I. Moser</td>
      <td>Ã…lesund</td>
      <td>Norway</td>
      <td>Male</td>
      <td>Norwegian University of Science and Technology...</td>
      <td>Trondheim</td>
      <td>Norway</td>
      <td>57</td>
      <td>52</td>
    </tr>
    <tr>
      <th>856</th>
      <td>2014</td>
      <td>Peace</td>
      <td>2-Jan</td>
      <td>913</td>
      <td>Kailash Satyarthi</td>
      <td>Vidisha</td>
      <td>India</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United Kingdom</td>
      <td>65</td>
      <td>60</td>
    </tr>
    <tr>
      <th>857</th>
      <td>2014</td>
      <td>Peace</td>
      <td>2-Jan</td>
      <td>914</td>
      <td>Malala Yousafzai</td>
      <td>Mingora</td>
      <td>Pakistan</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United States of America</td>
      <td>22</td>
      <td>17</td>
    </tr>
    <tr>
      <th>858</th>
      <td>2014</td>
      <td>Physics</td>
      <td>3-Jan</td>
      <td>906</td>
      <td>Isamu Akasaki</td>
      <td>Chiran</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Meijo University</td>
      <td>Nagoya</td>
      <td>Japan</td>
      <td>90</td>
      <td>85</td>
    </tr>
    <tr>
      <th>859</th>
      <td>2014</td>
      <td>Physics</td>
      <td>3-Jan</td>
      <td>907</td>
      <td>Hiroshi Amano</td>
      <td>Hamamatsu</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Nagoya University</td>
      <td>Nagoya</td>
      <td>Japan</td>
      <td>59</td>
      <td>54</td>
    </tr>
    <tr>
      <th>860</th>
      <td>2014</td>
      <td>Physics</td>
      <td>3-Jan</td>
      <td>908</td>
      <td>Shuji Nakamura</td>
      <td>Ikata</td>
      <td>Japan</td>
      <td>Male</td>
      <td>University of California</td>
      <td>Santa Barbara, CA</td>
      <td>United States of America</td>
      <td>65</td>
      <td>60</td>
    </tr>
    <tr>
      <th>861</th>
      <td>2015</td>
      <td>Chemistry</td>
      <td>3-Jan</td>
      <td>921</td>
      <td>Tomas Lindahl</td>
      <td>Stockholm</td>
      <td>Sweden</td>
      <td>Male</td>
      <td>Francis Crick Institute</td>
      <td>Hertfordshire</td>
      <td>United Kingdom</td>
      <td>81</td>
      <td>77</td>
    </tr>
    <tr>
      <th>862</th>
      <td>2015</td>
      <td>Chemistry</td>
      <td>3-Jan</td>
      <td>922</td>
      <td>Paul Modrich</td>
      <td>Raton, NM</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Howard Hughes Medical Institute</td>
      <td>Durham, NC</td>
      <td>United States of America</td>
      <td>73</td>
      <td>69</td>
    </tr>
    <tr>
      <th>863</th>
      <td>2015</td>
      <td>Chemistry</td>
      <td>3-Jan</td>
      <td>923</td>
      <td>Aziz Sancar</td>
      <td>Savur</td>
      <td>Turkey</td>
      <td>Male</td>
      <td>University of North Carolina</td>
      <td>Chapel Hill, NC</td>
      <td>United States of America</td>
      <td>73</td>
      <td>69</td>
    </tr>
    <tr>
      <th>864</th>
      <td>2015</td>
      <td>Economics</td>
      <td>1-Jan</td>
      <td>926</td>
      <td>Angus Deaton</td>
      <td>Edinburgh</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Princeton University</td>
      <td>Princeton, NJ</td>
      <td>United States of America</td>
      <td>74</td>
      <td>70</td>
    </tr>
    <tr>
      <th>865</th>
      <td>2015</td>
      <td>Literature</td>
      <td>1-Jan</td>
      <td>924</td>
      <td>Svetlana Alexievich</td>
      <td>Ivano-Frankivsk</td>
      <td>Ukraine</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United States of America</td>
      <td>71</td>
      <td>67</td>
    </tr>
    <tr>
      <th>866</th>
      <td>2015</td>
      <td>Medicine</td>
      <td>4-Jan</td>
      <td>916</td>
      <td>William C. Campbell</td>
      <td>Ramelton</td>
      <td>Ireland</td>
      <td>Male</td>
      <td>Drew University</td>
      <td>Madison, NJ</td>
      <td>United States of America</td>
      <td>89</td>
      <td>85</td>
    </tr>
    <tr>
      <th>867</th>
      <td>2015</td>
      <td>Medicine</td>
      <td>4-Jan</td>
      <td>917</td>
      <td>Satoshi ÅŒmura</td>
      <td>Yamanashi Prefecture</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Kitasato University</td>
      <td>Tokyo</td>
      <td>Japan</td>
      <td>84</td>
      <td>80</td>
    </tr>
    <tr>
      <th>868</th>
      <td>2015</td>
      <td>Medicine</td>
      <td>2-Jan</td>
      <td>918</td>
      <td>Youyou Tu</td>
      <td>Zhejiang Ningbo</td>
      <td>China</td>
      <td>Female</td>
      <td>China Academy of Traditional Chinese Medicine</td>
      <td>Beijing</td>
      <td>China</td>
      <td>89</td>
      <td>85</td>
    </tr>
    <tr>
      <th>869</th>
      <td>2015</td>
      <td>Physics</td>
      <td>2-Jan</td>
      <td>919</td>
      <td>Takaaki Kajita</td>
      <td>Higashimatsuyama</td>
      <td>Japan</td>
      <td>Male</td>
      <td>University of Tokyo</td>
      <td>Kashiwa</td>
      <td>Japan</td>
      <td>60</td>
      <td>56</td>
    </tr>
    <tr>
      <th>870</th>
      <td>2015</td>
      <td>Physics</td>
      <td>2-Jan</td>
      <td>920</td>
      <td>Arthur B. McDonald</td>
      <td>Sydney</td>
      <td>Canada</td>
      <td>Male</td>
      <td>Queen's University</td>
      <td>Kingston</td>
      <td>Canada</td>
      <td>76</td>
      <td>72</td>
    </tr>
    <tr>
      <th>871</th>
      <td>2016</td>
      <td>Chemistry</td>
      <td>3-Jan</td>
      <td>931</td>
      <td>Jean-Pierre Sauvage</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>University of Strasbourg</td>
      <td>Strasbourg</td>
      <td>France</td>
      <td>75</td>
      <td>72</td>
    </tr>
    <tr>
      <th>872</th>
      <td>2016</td>
      <td>Chemistry</td>
      <td>3-Jan</td>
      <td>932</td>
      <td>Sir J. Fraser Stoddart</td>
      <td>Edinburgh</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Northwestern University</td>
      <td>Evanston, IL</td>
      <td>United States of America</td>
      <td>77</td>
      <td>74</td>
    </tr>
    <tr>
      <th>873</th>
      <td>2016</td>
      <td>Chemistry</td>
      <td>3-Jan</td>
      <td>933</td>
      <td>Bernard L. Feringa</td>
      <td>Barger-Compascuum</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>University of Groningen</td>
      <td>Groningen</td>
      <td>Netherlands</td>
      <td>68</td>
      <td>65</td>
    </tr>
    <tr>
      <th>874</th>
      <td>2016</td>
      <td>Economics</td>
      <td>2-Jan</td>
      <td>935</td>
      <td>Oliver Hart</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Harvard University</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>71</td>
      <td>68</td>
    </tr>
    <tr>
      <th>875</th>
      <td>2016</td>
      <td>Economics</td>
      <td>2-Jan</td>
      <td>936</td>
      <td>Bengt HolmstrÃ¶m</td>
      <td>Helsinki</td>
      <td>Finland</td>
      <td>Male</td>
      <td>Massachusetts Institute of Technology (MIT)</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>70</td>
      <td>67</td>
    </tr>
    <tr>
      <th>876</th>
      <td>2016</td>
      <td>Literature</td>
      <td>1-Jan</td>
      <td>937</td>
      <td>Bob Dylan</td>
      <td>Duluth, MN</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>United States of America</td>
      <td>78</td>
      <td>75</td>
    </tr>
    <tr>
      <th>877</th>
      <td>2016</td>
      <td>Medicine</td>
      <td>1-Jan</td>
      <td>927</td>
      <td>Yoshinori Ohsumi</td>
      <td>Fukuoka</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Tokyo Institute of Technology</td>
      <td>Tokyo</td>
      <td>Japan</td>
      <td>74</td>
      <td>71</td>
    </tr>
    <tr>
      <th>878</th>
      <td>2016</td>
      <td>Peace</td>
      <td>1-Jan</td>
      <td>934</td>
      <td>Juan Manuel Santos</td>
      <td>BogotÃ¡</td>
      <td>Colombia</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Colombia</td>
      <td>68</td>
      <td>65</td>
    </tr>
    <tr>
      <th>879</th>
      <td>2016</td>
      <td>Physics</td>
      <td>2-Jan</td>
      <td>928</td>
      <td>David J. Thouless</td>
      <td>Bearsden</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>University of Washington</td>
      <td>Seattle, WA</td>
      <td>United States of America</td>
      <td>85</td>
      <td>82</td>
    </tr>
    <tr>
      <th>880</th>
      <td>2016</td>
      <td>Physics</td>
      <td>4-Jan</td>
      <td>929</td>
      <td>F. Duncan M. Haldane</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Princeton University</td>
      <td>Princeton, NJ</td>
      <td>United States of America</td>
      <td>68</td>
      <td>65</td>
    </tr>
    <tr>
      <th>881</th>
      <td>2016</td>
      <td>Physics</td>
      <td>4-Jan</td>
      <td>930</td>
      <td>J. Michael Kosterlitz</td>
      <td>Aberdeen</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Brown University</td>
      <td>Providence, RI</td>
      <td>United States of America</td>
      <td>76</td>
      <td>73</td>
    </tr>
  </tbody>
</table>
<p>882 rows × 13 columns</p>
</div>



 ### Statistics of the data


```python
data.describe()
data.groupby('Full Name').count().filter(n>1).arrange(desc(n))
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Age</th>
      <th>Age_Win</th>
    </tr>
    <tr>
      <th>Full Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A. Michael Spence</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Aage Niels Bohr</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Aaron Ciechanover</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Aaron Klug</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Abdus Salam</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Ada E. Yonath</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Adam G. Riess</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Adolf Friedrich Johann Butenandt</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Adolf Otto Reinhold Windaus</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Adolfo PÃ©rez Esquivel</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Ahmed H. Zewail</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Akira Suzuki</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Alan G. MacDiarmid</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Alan J. Heeger</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Alan Lloyd Hodgkin</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert Abraham Michelson</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert Arnold (Al) Gore Jr.</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert Camus</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert Claude</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert Einstein</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert Fert</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert John Lutuli</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert Schweitzer</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albert von Szent-Györgyi Nagyrápolt</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Albrecht Kossel</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Aleksandr Isayevich Solzhenitsyn</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Aleksandr Mikhailovich Prokhorov</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Alexei A. Abrikosov</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Alexis Carrel</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Alfonso GarcÃ­a Robles</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>William E. Moerner</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William F. Sharpe</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William Faulkner</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William Francis Giauque</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William Golding</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William H. Stein</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William Lawrence Bragg</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William N. Lipscomb</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William Parry Murphy</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William Randal Cremer</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William S. Knowles</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>William Vickrey</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Willis Eugene Lamb</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Willy Brandt</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Wislawa Szymborska</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Wladyslaw Stanislaw Reymont</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Wole Soyinka</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Wolfgang Ketterle</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Wolfgang Paul</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Wolfgang Pauli</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Yasser Arafat</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Yasunari Kawabata</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Yitzhak Rabin</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Yoichiro Nambu</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Yoshinori Ohsumi</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Youyou Tu</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Yuan T. Lee</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Yves Chauvin</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Zhores I. Alferov</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Élie Ducommun</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>878 rows × 12 columns</p>
</div>



### Current age distribution


```python
sns.distplot(data.Age, bins=35)
sns.set(rc={"figure.figsize": (10, 5)})
```


![png](DE_Project_files/DE_Project_4_0.png)


### The distribution of the age when the nobel prize was earned


```python
sns.distplot(data.Age_Win, bins=35)
sns.set(rc={"figure.figsize": (10, 5)})
```


![png](DE_Project_files/DE_Project_6_0.png)


### The distribution of the categories and their counts


```python
sns.countplot(y="Category", data=data,order=data.Category.value_counts().index,palette='deep')
sns.despine();
data['Category'].value_counts()
```




    Medicine      211
    Physics       204
    Chemistry     175
    Literature    113
    Peace         101
    Economics      78
    Name: Category, dtype: int64




![png](DE_Project_files/DE_Project_8_1.png)


### Gender Distribution 


```python
sns.countplot(x="Sex", data=data, palette='GnBu_d')
sns.despine();
data['Sex'].value_counts()
```




    Male      834
    Female     48
    Name: Sex, dtype: int64




![png](DE_Project_files/DE_Project_10_1.png)


### Organization Country Counts


```python
data['Organization Country'].value_counts()
```




    United States of America               413
    United Kingdom                         112
    France                                  58
    Germany                                 58
    Sweden                                  30
    Switzerland                             26
    Federal Republic of Germany             23
    Japan                                   19
    Union of Soviet Socialist Republics     16
    Denmark                                 13
    Netherlands                             12
    Belgium                                  9
    Norway                                   9
    Canada                                   7
    Israel                                   6
    Italy                                    6
    Austria                                  6
    Spain                                    6
    Australia                                5
    Russia                                   5
    Northern Ireland                         5
    Iran                                     2
    Colombia                                 2
    Liberia                                  2
    Chile                                    2
    Guatemala                                2
    Argentina                                2
    Portugal                                 2
    East Timor                               2
    Bulgaria                                 1
    Costa Rica                               1
    Nigeria                                  1
    Ghana                                    1
    Iceland                                  1
    Guadeloupe Island                        1
    Myanmar                                  1
    China                                    1
    Ireland                                  1
    Bangladesh                               1
    Zimbabwe                                 1
    Yemen                                    1
    Greece                                   1
    Hungary                                  1
    Kenya                                    1
    Finland                                  1
    Alsace (then Germany, now France)        1
    Peru                                     1
    India                                    1
    Trinidad                                 1
    Madagascar                               1
    Czechoslovakia                           1
    Name: Organization Country, dtype: int64



### Organization Country Distribution 


```python
sns.countplot(y="Organization Country", data=data,order=data["Organization Country"].value_counts().index,palette='deep')
sns.set(rc={"figure.figsize": (15, 12)})
```


![png](DE_Project_files/DE_Project_14_0.png)


### Birth Country Distribution


```python
sns.countplot(y="Birth Country", data=data,order=data["Birth Country"].value_counts().head(30).index,palette='deep')
sns.set(rc={"figure.figsize": (15, 12)})
```


![png](DE_Project_files/DE_Project_16_0.png)


### Corelation analysis between the organization country and the category


```python
contengency_table = pd.crosstab(data["Category"],data["Organization Country"], margins= False)
contengency_table

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Organization Country</th>
      <th>Alsace (then Germany, now France)</th>
      <th>Argentina</th>
      <th>Australia</th>
      <th>Austria</th>
      <th>Bangladesh</th>
      <th>Belgium</th>
      <th>Bulgaria</th>
      <th>Canada</th>
      <th>Chile</th>
      <th>China</th>
      <th>...</th>
      <th>Russia</th>
      <th>Spain</th>
      <th>Sweden</th>
      <th>Switzerland</th>
      <th>Trinidad</th>
      <th>Union of Soviet Socialist Republics</th>
      <th>United Kingdom</th>
      <th>United States of America</th>
      <th>Yemen</th>
      <th>Zimbabwe</th>
    </tr>
    <tr>
      <th>Category</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Chemistry</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>5</td>
      <td>6</td>
      <td>0</td>
      <td>1</td>
      <td>27</td>
      <td>74</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Economics</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>6</td>
      <td>62</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Literature</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>5</td>
      <td>7</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>10</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Medicine</th>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>4</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
      <td>31</td>
      <td>106</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Peace</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>2</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>0</td>
      <td>2</td>
      <td>13</td>
      <td>33</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Physics</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>7</td>
      <td>0</td>
      <td>7</td>
      <td>25</td>
      <td>99</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>6 rows × 51 columns</p>
</div>



#### From the above table it is shown that the USA is top country in all the categories followed by the UK 


```python
st.chi2_contingency(contengency_table)
```




    (416.7448150168042,
     1.773631129090637e-10,
     250,
     array([[1.98412698e-01, 3.96825397e-01, 9.92063492e-01, 1.19047619e+00,
             1.98412698e-01, 1.78571429e+00, 1.98412698e-01, 1.38888889e+00,
             3.96825397e-01, 1.98412698e-01, 3.96825397e-01, 1.98412698e-01,
             1.98412698e-01, 2.57936508e+00, 3.96825397e-01, 4.56349206e+00,
             1.98412698e-01, 1.15079365e+01, 1.15079365e+01, 1.98412698e-01,
             1.98412698e-01, 1.98412698e-01, 3.96825397e-01, 1.98412698e-01,
             1.98412698e-01, 1.98412698e-01, 3.96825397e-01, 1.98412698e-01,
             1.19047619e+00, 1.19047619e+00, 3.76984127e+00, 1.98412698e-01,
             3.96825397e-01, 1.98412698e-01, 1.98412698e-01, 2.38095238e+00,
             1.98412698e-01, 9.92063492e-01, 1.78571429e+00, 1.98412698e-01,
             3.96825397e-01, 9.92063492e-01, 1.19047619e+00, 5.95238095e+00,
             5.15873016e+00, 1.98412698e-01, 3.17460317e+00, 2.22222222e+01,
             8.19444444e+01, 1.98412698e-01, 1.98412698e-01],
            [8.84353741e-02, 1.76870748e-01, 4.42176871e-01, 5.30612245e-01,
             8.84353741e-02, 7.95918367e-01, 8.84353741e-02, 6.19047619e-01,
             1.76870748e-01, 8.84353741e-02, 1.76870748e-01, 8.84353741e-02,
             8.84353741e-02, 1.14965986e+00, 1.76870748e-01, 2.03401361e+00,
             8.84353741e-02, 5.12925170e+00, 5.12925170e+00, 8.84353741e-02,
             8.84353741e-02, 8.84353741e-02, 1.76870748e-01, 8.84353741e-02,
             8.84353741e-02, 8.84353741e-02, 1.76870748e-01, 8.84353741e-02,
             5.30612245e-01, 5.30612245e-01, 1.68027211e+00, 8.84353741e-02,
             1.76870748e-01, 8.84353741e-02, 8.84353741e-02, 1.06122449e+00,
             8.84353741e-02, 4.42176871e-01, 7.95918367e-01, 8.84353741e-02,
             1.76870748e-01, 4.42176871e-01, 5.30612245e-01, 2.65306122e+00,
             2.29931973e+00, 8.84353741e-02, 1.41496599e+00, 9.90476190e+00,
             3.65238095e+01, 8.84353741e-02, 8.84353741e-02],
            [1.28117914e-01, 2.56235828e-01, 6.40589569e-01, 7.68707483e-01,
             1.28117914e-01, 1.15306122e+00, 1.28117914e-01, 8.96825397e-01,
             2.56235828e-01, 1.28117914e-01, 2.56235828e-01, 1.28117914e-01,
             1.28117914e-01, 1.66553288e+00, 2.56235828e-01, 2.94671202e+00,
             1.28117914e-01, 7.43083900e+00, 7.43083900e+00, 1.28117914e-01,
             1.28117914e-01, 1.28117914e-01, 2.56235828e-01, 1.28117914e-01,
             1.28117914e-01, 1.28117914e-01, 2.56235828e-01, 1.28117914e-01,
             7.68707483e-01, 7.68707483e-01, 2.43424036e+00, 1.28117914e-01,
             2.56235828e-01, 1.28117914e-01, 1.28117914e-01, 1.53741497e+00,
             1.28117914e-01, 6.40589569e-01, 1.15306122e+00, 1.28117914e-01,
             2.56235828e-01, 6.40589569e-01, 7.68707483e-01, 3.84353741e+00,
             3.33106576e+00, 1.28117914e-01, 2.04988662e+00, 1.43492063e+01,
             5.29126984e+01, 1.28117914e-01, 1.28117914e-01],
            [2.39229025e-01, 4.78458050e-01, 1.19614512e+00, 1.43537415e+00,
             2.39229025e-01, 2.15306122e+00, 2.39229025e-01, 1.67460317e+00,
             4.78458050e-01, 2.39229025e-01, 4.78458050e-01, 2.39229025e-01,
             2.39229025e-01, 3.10997732e+00, 4.78458050e-01, 5.50226757e+00,
             2.39229025e-01, 1.38752834e+01, 1.38752834e+01, 2.39229025e-01,
             2.39229025e-01, 2.39229025e-01, 4.78458050e-01, 2.39229025e-01,
             2.39229025e-01, 2.39229025e-01, 4.78458050e-01, 2.39229025e-01,
             1.43537415e+00, 1.43537415e+00, 4.54535147e+00, 2.39229025e-01,
             4.78458050e-01, 2.39229025e-01, 2.39229025e-01, 2.87074830e+00,
             2.39229025e-01, 1.19614512e+00, 2.15306122e+00, 2.39229025e-01,
             4.78458050e-01, 1.19614512e+00, 1.43537415e+00, 7.17687075e+00,
             6.21995465e+00, 2.39229025e-01, 3.82766440e+00, 2.67936508e+01,
             9.88015873e+01, 2.39229025e-01, 2.39229025e-01],
            [1.14512472e-01, 2.29024943e-01, 5.72562358e-01, 6.87074830e-01,
             1.14512472e-01, 1.03061224e+00, 1.14512472e-01, 8.01587302e-01,
             2.29024943e-01, 1.14512472e-01, 2.29024943e-01, 1.14512472e-01,
             1.14512472e-01, 1.48866213e+00, 2.29024943e-01, 2.63378685e+00,
             1.14512472e-01, 6.64172336e+00, 6.64172336e+00, 1.14512472e-01,
             1.14512472e-01, 1.14512472e-01, 2.29024943e-01, 1.14512472e-01,
             1.14512472e-01, 1.14512472e-01, 2.29024943e-01, 1.14512472e-01,
             6.87074830e-01, 6.87074830e-01, 2.17573696e+00, 1.14512472e-01,
             2.29024943e-01, 1.14512472e-01, 1.14512472e-01, 1.37414966e+00,
             1.14512472e-01, 5.72562358e-01, 1.03061224e+00, 1.14512472e-01,
             2.29024943e-01, 5.72562358e-01, 6.87074830e-01, 3.43537415e+00,
             2.97732426e+00, 1.14512472e-01, 1.83219955e+00, 1.28253968e+01,
             4.72936508e+01, 1.14512472e-01, 1.14512472e-01],
            [2.31292517e-01, 4.62585034e-01, 1.15646259e+00, 1.38775510e+00,
             2.31292517e-01, 2.08163265e+00, 2.31292517e-01, 1.61904762e+00,
             4.62585034e-01, 2.31292517e-01, 4.62585034e-01, 2.31292517e-01,
             2.31292517e-01, 3.00680272e+00, 4.62585034e-01, 5.31972789e+00,
             2.31292517e-01, 1.34149660e+01, 1.34149660e+01, 2.31292517e-01,
             2.31292517e-01, 2.31292517e-01, 4.62585034e-01, 2.31292517e-01,
             2.31292517e-01, 2.31292517e-01, 4.62585034e-01, 2.31292517e-01,
             1.38775510e+00, 1.38775510e+00, 4.39455782e+00, 2.31292517e-01,
             4.62585034e-01, 2.31292517e-01, 2.31292517e-01, 2.77551020e+00,
             2.31292517e-01, 1.15646259e+00, 2.08163265e+00, 2.31292517e-01,
             4.62585034e-01, 1.15646259e+00, 1.38775510e+00, 6.93877551e+00,
             6.01360544e+00, 2.31292517e-01, 3.70068027e+00, 2.59047619e+01,
             9.55238095e+01, 2.31292517e-01, 2.31292517e-01]]))



- p-value = 1.773631129090637e-10 which is <0.05 the hypothesis is rejected.
- So the attributes are corelated. In other word there is a relation between the organization country and the category 

### Acording to the previous statistics:
##### The mean age for winning a nobel rize is 59 
##### Medicin is the top category in which nobel prize is won
##### USA is the top country from which nobel prize winners come from either born there or belong to an organization there
##### Nobel prize winners are mosly males 



```python

```
