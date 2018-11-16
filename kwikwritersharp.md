---
layout: page
permalink: /portfolio/samples/kwikwritersharp/
---

# KwikWriter back end and front end

This is a partial look at the back end and front end for KwikWriter as it stands.

This project is still a Work in progress and it is licensed under GPL-3. You can also check the project our on [GitHub](https://github.com/netgibbon/KwikWrite). I am plannig to remove the un-needed references at the completion of the project.

# Main Window back end
{% highlight csharp %}
/*
    KwikWriter  Copyright (C) 2017  Michael Laws
    This program comes with ABSOLUTELY NO WARRANTY; for details type `show w'.
    This is free software, and you are welcome to redistribute it
 */

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;
using System.Windows.Threading;

namespace KwikWrite
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {    
  
        public MainWindow()
        {
            InitializeComponent();
            //Initialise a timer to do a task every 10 milliseconds
            DispatcherTimer millisecondUpdator = new DispatcherTimer();
            millisecondUpdator.Interval = TimeSpan.FromMilliseconds(10);
            millisecondUpdator.Tick += timer_Tick;
            millisecondUpdator.Start();
          
            
            void timer_Tick(object sender, EventArgs e)
            {
                //Update the clock
                timeLabel.Content = DateTime.Now.ToShortTimeString();

                //see if the caps lock, num lock and scroll lock keys are pressed
                if (Console.CapsLock)
                {
                    capsNotifier.Foreground = Brushes.White;
                }
                else
                {
                    capsNotifier.Foreground = Brushes.DarkSlateGray;
                }

                if (Console.NumberLock)
                {
                    numLockNotifier.Foreground = Brushes.White;
                }
                else
                {
                    numLockNotifier.Foreground = Brushes.DarkSlateGray;
                }
            }

            
        }

        private void quitButton_Click(object sender, RoutedEventArgs e)
        {
            Application.Current.Shutdown();
        }

        private void helpButton_Click(object sender, RoutedEventArgs e)
        {
            //"Create" the help window
            HelpScreen helpScreen = new HelpScreen();
            helpScreen.Show();
        }
    }
}
{% endhighlight %}

# XAML front end

{% highlight xml %}
<!--
    KwikWriter  Copyright (C) 2017  Michael Laws
    This program comes with ABSOLUTELY NO WARRANTY; for details type `show w'.
    This is free software, and you are welcome to redistribute it
-->
    
    <Window x:Class="KwikWrite.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:KwikWrite"
        mc:Ignorable="d"
        Title="KwikWriter" Height="{DynamicResource {x:Static SystemParameters.FullPrimaryScreenHeightKey}}" Width="{DynamicResource {x:Static SystemParameters.FullPrimaryScreenWidthKey}}" ResizeMode="NoResize" WindowStartupLocation="CenterScreen" WindowState="Maximized" Background="#FFBBBBBB" WindowStyle="None">
    <Grid>
        <!--Left to right-->
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="6*"/>
            <ColumnDefinition Width="2*"/>
        </Grid.ColumnDefinitions>

        <!--Top to bottom-->
        <Grid.RowDefinitions>
            <RowDefinition Height="0.5*"/>
            <RowDefinition Height="9*"/>
            <RowDefinition Height="0.3*"/>
        </Grid.RowDefinitions>
        
        <!--Top and bottom bars for logos, buttons and status info-->
        <Rectangle x:Name="topBar" Fill="Black" Grid.Column="0" Grid.Row="0" Grid.ColumnSpan="3"></Rectangle>
        <Rectangle x:Name="bottomBar" Fill="Black" Grid.Column="0" Grid.Row="2" Grid.ColumnSpan="3"></Rectangle>
        
        <!--Rich Text Box with Spellcheck and proper styling-->
        <RichTextBox x:Name="textBox" Grid.Column="1" Grid.Row="1" SpellCheck.IsEnabled="True" SelectionBrush="#FF00D7B0" BorderBrush="{x:Null}" FontSize="16" IsDocumentEnabled="True" AcceptsTab="True" FontFamily="Arial">
            <RichTextBox.Effect>
                <DropShadowEffect BlurRadius="12" Direction="270"/>
            </RichTextBox.Effect>
        </RichTextBox>

        <!--Top bar label-->
        <Label Content="KwikWriter" Grid.Column="0" Grid.Row="0" Foreground="White" FontWeight="Bold" FontSize="24" VerticalAlignment="Center" HorizontalAlignment="left" Margin="20,0,0,0"></Label>
        <!--Top bar buttons-->
        <Button x:Name="helpButton" Grid.Column="3" Content="?" Foreground="White" Background="Black" BorderBrush="White" FontSize="16px" Width="30px" Height="30px" HorizontalAlignment="right" Margin="0,0,50,0" Click="helpButton_Click" ></Button>
        <Button x:Name="quitButton" Grid.Column="3" Content="X" Foreground="White" Background="Black" BorderBrush="White" FontSize="16px" Width="30px" Height="30px" HorizontalAlignment="Right" Margin="0,0,10,0" Click="quitButton_Click"></Button>
        
        <!--Bottom bar timer-->
        <Label x:Name="timeLabel" Grid.Column="3" Grid.Row="3" Foreground="White" Content="Changeme" HorizontalAlignment="Right" VerticalAlignment="Top" Margin="0,0,50,-10"></Label>
        
        <!--Bottom bar system key notifiers-->
        <Label x:Name="capsNotifier" Grid.Column="0" Grid.Row="3" Foreground="DarkSlateGray" Content="CAPS" FontSize="12" Margin="20,0,0,-10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Label x:Name="numLockNotifier" Grid.Column="0" Grid.Row="3" Foreground="DarkSlateGray" Content="NUM" FontSize="12" Margin="80,0,0,-10" HorizontalAlignment="left" VerticalAlignment="Top"/>
    </Grid>
</Window>
{% endhighlight %}