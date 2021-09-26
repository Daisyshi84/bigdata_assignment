"""
DSP 439: Assignment 4
NAME: Minchan Shi
Discription:  when run using the command line in this cript, outputs the linguistic complexity of each
sequence in a file of sequences and shows the plots. If you want to run the different file,
you need change the file in the main function.
"""
import sys 
import pandas as pd
import matplotlib.pyplot as plt

def Kmers_funct(k):
    result = []
    for i in range(len(seq)-k+1):
        result.append(seq[i:i+k])
    return print('The number of kmers observed:',len(result),'\nThe sequence is:',result)


def count_observed_kmer(Seq,k):
    """
    Summary line: Counts the number of observed kmers for a sequence, for a specific k value
    
    Extended description: This function takes k and a sequence Seq as input arguments and determines the number of observed k-mers 
    for a particular k-value
    
    Parameters: 
    Seq: the input sequence for which the k-mers need to be determined
    k: the input value, ranges from 1 to length of sequence 
    
    Return:
    obs_kmers: the number of kmers observed
    """
    lk_pos = []
    for i in range(0,len(Seq)-k+1):
        lk_pos.append(Seq[i:i+k])

    lk_obs = list(set(lk_pos))
    return len(lk_obs)
    

def count_pos_kmer(Seq,k):
    """
    Summary line: Counts the number of possible kmers for a sequence, for a specific k value
    
    Extended description: This function takes k and a sequence Seq as input arguments and determines the number of possible/expected k-mers 
    for a particular k-value
    
    Parameters: 
    Seq: the input sequence for which the number of k-mers need to be determined
    k: the input value, ranges from 1 to length of the sequence 
    
    Return:
    pos_kmers: the number of kmers possible
    """
    
    lk_pos = []
    if k == 1:
        return 4;
    else:
        for i in range(0,len(Seq)-k+1):
            lk_pos.append(Seq[i:i+k])    
        return len(lk_pos)
    
def create_kmer_df(Seq):
    """
    Summary line: Creates a data frame with k values, observed number of kmers and possible number of kmers as columns. 
    
    Extended description: This function takes a sequence Seq as input argument and returns a pandas data frame 
    which has columns k value, observed kmer count and possible kmer count.
        
    Parameters: 
    Seq: the input sequence for which the kmer data frame needs to be created
    
    Return:
    kmer_df: kmer data drame
    """

    k = []
    k_pos_list = []
    k_obs_list = []
    k = (range(1,len(Seq)+1))
    
    for i in k:
        k_pos = count_pos_kmer(Seq,i)
        k_pos_list.append(k_pos)
    
    for i in k:
        k_obs = count_observed_kmer(Seq,i)
        k_obs_list.append(k_obs)
    
    kmer_df = pd.DataFrame(     # create the data frame
    {
         'k':k,
         'Observed kmers':k_obs_list,
         'Possible kmers':k_pos_list
    }
    )
    return kmer_df




def plot_kmer_prop(Seq):
    """
    Summary line: Creates a graph for proportion of each observed kmer
    
    Extended description: This function takes a sequence Seq as input argument and produces a plot for the proportion of each observed kmers 
    with respect to the number of possible kmers
        
    Parameters: 
    Seq: the input sequence for which the kmer proportion graph needs to be created
    
    Return:
    None
    """
    
    kmer_df = create_kmer_df(Seq)
    kmer_prop = kmer_df['Observed kmers']/kmer_df['Possible kmers'] # calculate the kmer proportion
    plt.plot(kmer_df['k'],kmer_prop)
    plt.title('Proportion of Observed kmers')
    plt.xlabel('k value')
    plt.ylabel('Observed/Possible kmers')
    plt.show()
    
def linguistic_complexity(Seq):
    """
    Summary line: Calculates the lingusitic complexity of a given sequence
    
    Extended description: This function takes a sequence Seq as input argument and produces the linguistic complexity, 
    the proportion of k-mers that are observed compared to the total number that are theoretically possible
        
    Parameters: 
    Seq: the input sequence for which the linguistic complexity needs to be determined
    
    Return:
    lc: linguistic complexity
    """
    
    kmer_df = create_kmer_df(Seq)
    tot_obs_kmer = sum(kmer_df['Observed kmers'])
    tot_pos_kmer = sum(kmer_df['Possible kmers'])
    try:
        lc = tot_obs_kmer/tot_pos_kmer
    except ZeroDivisionError:
        lc = 0
    return lc

if __name__=='__main__':
    
    file = "Python/file.txt"  # user shall input the file location here
    
    with open(file,'r') as f:
        text = f.read()
    text = text.split() 
    
    for i in range(len(text)):
        print("sequences is:",text[i])
        lc_seq = linguistic_complexity(text[i])
        print('\nlinguistic complexity is:',lc_seq) 
        plot_kmer_prop(text[i])


# 

# In[ ]:





# In[ ]:


