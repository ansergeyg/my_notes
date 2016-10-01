# my_notes
Everything about everything

Yesterday I finally implemented a small functionality concerning displaying language specifiy data in our custom combobox inerited from devexpress combobox.

My team lead told me that you I should create a specific property in a corresponding class which will be used in a TextField property of the combobox. So I did. And it didn't work. I wasted a day to make it work but couldn't succeed. Finally I asked our team lead for help and we stared debuging together at my computer. After a long debugging the team lead figured this out.

The custom combobox could only work if there certain fields were set properly. And also, the datasource control for this combobox should also have a certain property to be set. My team lead recogninzed that so we properly set my combobox and everything started working fine.

The point is that I myself tried to set various propeties to make the combobox work but was not luckiy.  If I found the same a similar combobox with a similar datasource and looked carefully at the property settings I would have done the same settings for my combobox and make it work. Things aren't alwasys natural:)
