#### Data Modelling

![Bookmy_show_assignment](https://user-images.githubusercontent.com/10736692/282352348-96b497dd-ee0e-416f-81d8-e06f552ac908.png)


#### Database Definition (DBML)
```bash

 Table user {
  id integer [primary key, increment]
  name varchar [not null]
  email varchar [unique, not null]
  mobile integer [unique, not null]
  password varchar [not null]
  created_at timestamp [default: 'now()']
  updated_at timestamp [default: 'now()']
}

Table booking {
  id integer [primary key, increment]
  booking_date timestamp [not null]
  showtime_id integer [not null]
  user_id integer [not null]
  no_of_tickets integer [not null]
  seat_no varchar [not null]
  tot_amount integer [not null]
  created_at timestamp [default: 'now()']
  updated_at timestamp [default: 'now()']
}

Table theater {
  id integer [primary key, increment]
  theater_name varchar [not null, unique]
  city varchar
  no_of_screens integer [not null]
  created_at timestamp [default: 'now()']
  updated_at timestamp [default: 'now()']
}

Table screen {
  id integer [primary key, increment]
  screen_name varchar [unique, not null]
  theater_id integer [not null]
  created_at timestamp [default: 'now()']
  updated_at timestamp [default: 'now()']
}

Table movie {
  id integer [primary key, increment]
  movie_name varchar [not null]
  language varchar [default: 'English']
  created_at timestamp [default: 'now()']
  updated_at timestamp [default: 'now()']
}

Table movie_info {
  id integer [primary key, increment]
  movie_id integer [not null]
  director varchar
  certificate varchar [default: 'U']
  genere varchar
  release_date varchar [not null]
  format varchar [default: '2D']
  ticket_price integer
  created_at timestamp [default: 'now()']
  updated_at timestamp [default: 'now()']
}

Table showtime {
  id integer [primary key, increment]
  screen_id integer [not null]
  start_time timestamp [not null]
  end_time timestamp [not null]
  date date
  movie_id integer [not null]
  theater_id integer [not null]
  created_at timestamp [default: 'now()']
  updated_at timestamp [default: 'now()']
}

Ref: theater.id < screen.theater_id // one to many

Ref: theater.id < showtime.theater_id // one to many

Ref: screen.id < showtime.screen_id // one to many

Ref: movie.id < showtime.movie_id // one to many

Ref: movie.id - movie_info.movie_id // one to one

Ref: user.id < booking.user_id // one to many

Ref: showtime.id < booking.showtime_id // one to many
```


#### Database creation 

```sql
  CREATE DATABASE IF NOT EXISTS `bookmyshow`;
 ```

```sql
  use `bookmyshow`;
```

#### Table Creation

```sql
CREATE TABLE `user` (
  `id` integer PRIMARY KEY AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `email` varchar(255) UNIQUE NOT NULL,
  `mobile` varchar(20) UNIQUE NOT NULL,
  `password` varchar(255) NOT NULL,
  `created_at` timestamp DEFAULT now(),
  `updated_at` timestamp DEFAULT now()
);
```

```sql
CREATE TABLE `booking` (
  `id` integer PRIMARY KEY AUTO_INCREMENT,
  `booking_date` timestamp NOT NULL,
  `showtime_id` integer NOT NULL,
  `user_id` integer NOT NULL,
  `no_of_tickets` integer NOT NULL,
  `seat_no` varchar(255) NOT NULL,
  `tot_amount` integer NOT NULL,
  `created_at` timestamp DEFAULT now(),
  `updated_at` timestamp DEFAULT now()
);
```

```sql
CREATE TABLE `theater` (
  `id` integer PRIMARY KEY AUTO_INCREMENT,
  `theater_name` varchar(255) UNIQUE NOT NULL,
  `city` varchar(255),
  `no_of_screens` integer NOT NULL,
  `created_at` timestamp DEFAULT now(),
  `updated_at` timestamp DEFAULT now()
);
```

```sql
CREATE TABLE `screen` (
  `id` integer PRIMARY KEY AUTO_INCREMENT,
  `screen_name` varchar(255) UNIQUE NOT NULL,
  `theater_id` integer NOT NULL,
  `created_at` timestamp DEFAULT now(),
  `updated_at` timestamp DEFAULT now()
);
```

```sql
CREATE TABLE `movie` (
  `id` integer PRIMARY KEY AUTO_INCREMENT,
  `movie_name` varchar(255) NOT NULL,
  `language` varchar(255) DEFAULT "English",
  `created_at` timestamp DEFAULT now(),
  `updated_at` timestamp DEFAULT now()
);
```

```sql
CREATE TABLE `movie_info` (
  `id` integer PRIMARY KEY AUTO_INCREMENT,
  `movie_id` integer NOT NULL,
  `director` varchar(255),
  `certificate` varchar(255) DEFAULT "U",
  `genere` varchar(255),
  `release_date` varchar(255),
  `format` varchar(255) DEFAULT "2D",
  `ticket_price` integer,
  `created_at` timestamp DEFAULT now(),
  `updated_at` timestamp DEFAULT now()
);
```

```sql
CREATE TABLE `showtime` (
  `id` integer PRIMARY KEY AUTO_INCREMENT,
  `screen_id` integer NOT NULL,
  `start_time` timestamp NOT NULL,
  `end_time` timestamp NOT NULL,
  `date` date,
  `movie_id` integer NOT NULL,
  `theater_id` integer NOT NULL,
  `created_at` timestamp DEFAULT now(),
  `updated_at` timestamp DEFAULT now()
);
 ```
 
 
 #### Constraints for the Tables creatied

```sql
ALTER TABLE `screen` ADD FOREIGN KEY (`theater_id`) REFERENCES `theater` (`id`);
```

```sql
ALTER TABLE `showtime` ADD FOREIGN KEY (`theater_id`) REFERENCES `theater` (`id`);
```

```sql
ALTER TABLE `showtime` ADD FOREIGN KEY (`screen_id`) REFERENCES `screen` (`id`);
```

```sql
ALTER TABLE `showtime` ADD FOREIGN KEY (`movie_id`) REFERENCES `movie` (`id`);
```

```sql 
ALTER TABLE `movie_info` ADD FOREIGN KEY (`movie_id`) REFERENCES `movie` (`id`);
```

```sql
ALTER TABLE `booking` ADD FOREIGN KEY (`user_id`) REFERENCES `user` (`id`);
```

```sql
ALTER TABLE `booking` ADD FOREIGN KEY (`showtime_id`) REFERENCES `showtime` (`id`);
```


#### Inserting data to Tables

```sql
INSERT INTO `user` (`name`, `email`, `mobile`, `password`) 
VALUES 
  ('Luke Skywalker', 'luke@sw.com', 1234567890, 'password123'),
  ('Leia Organa', 'leia@sw.com', 9876543210, 'securepass');
```
 
 
```sql
INSERT INTO `theater` (`theater_name`, `city`, `no_of_screens`)
VALUES
  ('Cinepolis', 'Metropolis', 5),
  ('PVR Cinemas', 'Tatooine', 3),
  ('INOX', 'Gotham City', 8);
```

```sql
INSERT INTO `screen` (`screen_name`, `theater_id`)
VALUES
  ('Starlight', 2),
  ('Moonbeam',  2),
  ('Sunflower',  2),
  ('Majestic Max', 1),
  ('Valet Vision',  1),
  ('Silver Screen',  1);    
```  
  
  
```sql
INSERT INTO `movie` (`movie_name`, `language`)
VALUES
  ('Superman Legacy', 'English'),
  ('The Empire Strikes Back',  'English'),
  ('Cinema Paradiso',  'Italian'),
  ('Los Cronocrímenes', 'Spanish');
```  
  
  
```sql
INSERT INTO `movie_info` (`movie_id`, `director`, `release_date`, `genere`, `certificate`, `format`, `ticket_price`)
VALUES
  ('1', 'James Gunn', '11-07-2025',  'Sci-fi/Adventure', 'U/A', '3D', '600'),
  ('2', 'Irvin Kershner', '21-05-1980', 'Sci-fi/Adventure', 'U/A', '3D', '600'),
  ('3',  'Giuseppe Tornatore', '17-11-1988', 'Drama/Mystery', 'A', '2D', '400'),
  ('4', 'Nacho Vigalondo', '16-11-2007', 'Sci-fi/Thriller', 'A', '2D', '300');
```  
  
  
```sql
INSERT INTO `showtime` (`screen_id`, `start_time`, `end_time`, `date`, `movie_id`, `theater_id`)
VALUES
  ('1', '2023-11-10 10:00:00', '2023-11-10 12:00:00', '2023-11-10',  '2', '2'),
  ('1', '2023-11-10 15:00:00', '2023-11-10 17:00:00', '2023-11-10',  '2', '2'),
  ('2', '2023-11-10 10:00:00', '2023-11-10 12:30:00', '2023-11-10',  '3', '2'),
  ('2', '2023-11-10 14:30:00', '2023-11-10 17:00:00', '2023-11-10',  '3', '2'),
  ('1', '2023-11-10 10:00:00', '2023-11-11 12:00:00', '2023-11-11',  '2', '2'),
  ('1', '2023-11-10 15:00:00', '2023-11-11 17:00:00', '2023-11-11',  '2', '2'),
  ('2', '2023-11-10 10:00:00', '2023-11-11 12:30:00', '2023-11-11',  '3', '2'),
  ('2', '2023-11-10 14:30:00', '2023-11-11 17:00:00', '2023-11-11',  '3', '2'),
  ('1', '2023-11-10 10:00:00', '2023-11-11 12:00:00', '2023-11-11',  '2', '1'),
  ('1', '2023-11-10 15:00:00', '2023-11-11 17:00:00', '2023-11-11',  '2', '1'),
  ('2', '2023-11-10 10:00:00', '2023-11-11 12:30:00', '2023-11-11',  '3', '1'),
  ('2', '2023-11-10 14:30:00', '2023-11-11 17:00:00', '2023-11-11',  '3', '1');
```  
  
  
```sql
INSERT INTO `booking` (`booking_date`, `showtime_id`, `user_id`, `no_of_tickets`, `seat_no`, `tot_amount`) 
VALUES 
  ('2023-11-15 08:30:00', 101, 1, 2, 'A1,A2', COALESCE((SELECT ticket_price FROM movie_info WHERE movie_id = 1),0) * 2);
```  

#### Write a query to list down all the shows on a given date at a given theatre along with their respective show timings. 

```sql
SELECT movie.movie_name, movie.language, showtime.start_time, 
showtime.end_time, theater.theater_name, movie_info.director, 
movie_info.genere
FROM showtime 
INNER JOIN movie ON showtime.movie_id = movie.id 
INNER JOIN theater ON showtime.theater_id = theater.id
INNER JOIN movie_info ON movie_info.movie_id = movie.id
WHERE showtime.date BETWEEN '2023-11-10' AND '2023-11-11'
AND theater_name = 'Cinepolis';
```


 