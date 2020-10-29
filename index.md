# Portfolio
Professional Portfolio 
#### Welcome to my professional code portfolio, below are some samples of code that I have either created myself, or made a contribution.
#### If you have any questions please contact me at [jl499774@dal.ca](mailto:jl499774@dal.ca)

## Importing Data
#### Firstly, this is how I read in Data, where "spid" represents the beginning of the filename
files = glob('spid*/*_data.txt')
dataframes = [pd.read_csv(x, sep='\t') for x in files]

## Cleaning data
##### Here is code to remove items from a data frame
df = df[df.block != 'practice']

##### Here is a loop with several cleaning steps used to process imported data that I helped write in a project. 
processed_dataframes = []
fast_list = []
slow_list = []
for file in files:
    stats_dict = {}
    unprocessed_dataframe = pd.read_csv(file, sep='\t')
    unprocessed_dataframe = unprocessed_dataframe.dropna()
    unprocessed_dataframe = unprocessed_dataframe.drop_duplicates(subset=['rt'])
    unprocessed_dataframe = unprocessed_dataframe[unprocessed_dataframe.block != 'practice']
    std_rt = unprocessed_dataframe['rt'].std()
    mean_rt = unprocessed_dataframe['rt'].mean()
    upperoutlier = mean_rt + std_rt*2
    loweroutlier = mean_rt - std_rt*2
    slow = unprocessed_dataframe.loc[unprocessed_dataframe['rt'] > upperoutlier]
    unprocessed_dataframe.loc[slow.index, 'rt'] = mean_rt
    fast = unprocessed_dataframe.loc[unprocessed_dataframe['rt'] < loweroutlier]
    unprocessed_dataframe.loc[fast.index, 'rt'] = mean_rt
    processed_dataframes.append(unprocessed_dataframe)
    fast_list.append(fast)
    slow_list.append(slow)
    
#### To further organize data, I am able to combine data into 2D NumPY arrays
np_err = np.array(err)
dat = np.array((np_rt_ms, np_err))
print(dat)

## Manipulating Data

#### I am able to use functions and methods to describe data
ean_rt= np.mean(rt_conversion).round(1)
print("Mean RT = " + str(mean_rt) + " ms")

std_rt = np.std(rt_conversion).round(2) 
print("Standard deviation = " + str(std_rt) + " ms")


#### I am also capable of writing and using loops
for index in more_rt:
    rt.append(index)
for index in more_err: 
    err.append(index)

## Data visualization 

#### Here's an example of code used to visualize data 
log_incon = df.loc[df['flankers'] == 'incongruent']
log_rt_incon = np.log(log_incon['rt'])

log_con = df.loc[df['flankers'] == 'congruent']
log_rt_con = np.log(log_con['rt'])

datas = {'congruent' : log_rt_con, 'incongruent' : log_rt_incon}
logcond = pd.DataFrame(data=datas)

logcond.plot(kind='box')
plt.show()


