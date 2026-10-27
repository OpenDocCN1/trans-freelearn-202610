```
<TextBox x:Name="TitleNameTextbox"
HorizontalAlignment="Left" Height="23"
Margin="169,131,0,0" TextWrapping="Wrap"
VerticalAlignment="Top" Width="311" Text="{Binding Title}" TabIndex="4"/>

<Label Content="Controller Name"
HorizontalAlignment="Left" Margin="36,103,0,0"
VerticalAlignment="Top"
RenderTransformOrigin="0.211,-0.154"/>

<TextBox x:Name="ViewNameTextbox" Text="{Binding ViewName}" HorizontalAlignment="Left" Height="23"
Margin="169,75,0,0" TextWrapping="Wrap"
VerticalAlignment="Top" Width="311" TabIndex="2"/>

<Label Content="Title" HorizontalAlignment="Left"
Margin="36,131,0,0" VerticalAlignment="Top"/>

<ComboBox HorizontalAlignment="Left"
Margin="169,46,0,0" VerticalAlignment="Top"
Width="311" ItemsSource="{Binding ProjectDBContexts}" SelectedItem="{Binding SelectedDbContext, Mode=OneWayToSource}"
DisplayMemberPath="DisplayName"/>

<Label Content="DBContext"
HorizontalAlignment="Left" Margin="36,46,0,0"
VerticalAlignment="Top" Width="128"/>

</Grid>
</Window>
```

19. The preceding XAML markup will bind the form controls to the CustomViewModel class. The final design for the form should resemble the following screenshot:

![](bbox=[0.24, 0.62, 0.76, 0.8])