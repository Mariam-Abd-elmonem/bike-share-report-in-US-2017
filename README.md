# bike-share-report-in-US-2017
#An interactive code..based on asking the user which city to ask about its data and I tried to avoid user common mistakes..this code was a graduation project from the first phase at professional track from udacity 









import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    while True :
        city = input("sir could you select a city from (chicago, new york city, washington) ? ").lower()
        if city not in (CITY_DATA) :
            print(" sorry this is invalid city")
        else :
            break

    # TO DO: get user input for month (all, january, february, ... , june)
    months = ("all","january","february","march","april","may","june")
    while True:
        month = input("please select month (all, january, february, ... , june)").lower()
        if month in months :
            break
        else :
            print("sorry sir invalid month")
    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    while True:
        days = ("all","monday","tuesday","wednesday","thursday","friday","saturday","sunday")
        day = input("please select a day of week (all, monday, tuesday, ... sunday)")
        if day in days :
            break
        else :
            print("sorry sir invalid day of week")


    print('-'*40)
    return city, month, day


def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    # load data file into a dataframe
    df = pd.read_csv(CITY_DATA[city])

    # convert the Start Time column to datetime
    df['Start Time'] = pd.to_datetime(df['Start Time'])

    # extract month and day of week from Start Time to create new columns
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.weekday_name

    # filter by month if applicable
    if month != 'all':
        # use the index of the months list to get the corresponding int
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1

        # filter by month to create the new dataframe
        df = df[df['month'] == month]

    # filter by day of week if applicable
    if day != 'all':
        # filter by day of week to create the new dataframe
        df = df[df['day_of_week'] == day.title()]


    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    most_month = df["month"].mode()[0]
    print("the most common month is: ",most_month)

    # TO DO: display the most common day of week
    most_day = df["day_of_week"].mode()[0]
    print("the most common say of week is : ",most_day )

    # TO DO: display the most common start hour
    most_hour = df["Start Time"].mode()[0]
    print("this is the most common start hour : ", most_hour)
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    most_start_station = df["Start Station"].mode()[0]
    print("this is the most commonly used start station : ", most_start_station) 

    # TO DO: display most commonly used end station
    most_end_station = df["End Station"].mode()[0]
    print("this is the most commonly End station : ",most_end_station)

    # TO DO: display most frequent combination of start station and end station trip
    combination_stations = (df["Start Station"]+ "," + df["End Station"]).mode()[0]
    print("this is the most frequent combination of start station and end station trip : ",combination_stations)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    print("this is the total travel time is : ",df["Trip Duration"].sum())

    # TO DO: display mean travel time
    print("this is the mean travel time is : ",df["Trip Duration"].mean())

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    user_types = df['User Type'].value_counts()
    print(" this is the count of user types : ",user_types)

    # TO DO: Display counts of gender
    if "Gender" in df:
        print("the counts of gender is: ",df["Gender"].value_counts())

    # TO DO: Display earliest, most recent, and most common year of birth
    if "Birth Year" in df:
        print("this is the most recent: ",df["Birth Year"].max())
        print("this is the earliest: ",df["Birth Year"].min())
        print("this is the most common year of birth : " ,df["Birth Year"].mode()[0])

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def display_data(df):
    print('/nRaw data is loading to check.../n')
    
    index = 0
    user_input= input("sir whould you like to display 5 raw data , type yes or no").lower()
    if user_input not in ["yes","no"]:
        print("sorry invalid choice , type yes or no ")
        user_input= input("sir whould you like to display 5 raw data , type yes or no").lower()
    elif user_input != "yes" :
        print("thank you")
    else :
        while index+5 < df.shape[0] :
            print(df.iloc[index:index+5])
            index+=5
            user_input= input("sir whould you like to display 5 more raw data , type yes or no").lower()
            if user_input != "yes" :
                print("thank you")
                break
       
           
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)
        display_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
