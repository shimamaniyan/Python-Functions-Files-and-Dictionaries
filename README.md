# Python-Functions-Files-and-Dictionaries
#Project1

punctuation_chars = ["'", '"', ",", ".", "!", ":", ";", '#', '@']
#creating a list of negative words
negative_words = []
with open("negative_words.txt", 'r') as p_words:
    for line in p_words:
        if line[0] != ';' and line[0] != '/n':
            negative_words.append(line.strip())            

#creating a list of positive words
positive_words = []
with open("positive_words.txt", 'r') as p_words:
    for line in p_words:
        if line[0] != ';' and line[0] != '/n':
            positive_words.append(line.strip())
   
#removing the punctuation marks in string
def strip_punctuation(s):
    for p in punctuation_chars:
        while p in s:
            s = s.replace(p, '')
    return s

#calculating the count of negative words
def get_neg(s):
    n_count = 0
    words = s.split(' ')
    for wrd in words:
        if strip_punctuation(wrd.lower().strip()) in negative_words:
            n_count += 1
    return n_count        

#calculating the count of positive words
def get_pos(s):
    p_count = 0
    words = s.split(' ')
    for wrd in words:
        if strip_punctuation(wrd.lower().strip()) in positive_words:
            p_count += 1
    return p_count

#calculating net score
def net_score(s):
    return get_pos(s) - get_neg(s)

#creating a new CSV file with the four columns
with open('project_twitter_data.csv', 'r') as input:
    with open('resulting_data.csv', 'w') as output:
        output.write('Number of Retweets, Number of Replies, Positive Score, Negative Score, Net Score')
        output.write('\n')
        for line in input.readlines()[1:]:
            p = line.split(',')
            row = '{},{},{},{},{}'.format(p[1].strip(), p[2].strip(), get_pos(p[0]), get_neg(p[0]), net_score(p[0]))
            output.write(row)
            output.write('\n')
            
#checking if CSF file is correct:
with open('resulting_data.csv', 'r') as file:
    print(file.read())
