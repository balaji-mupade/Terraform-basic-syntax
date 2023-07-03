# Configure the AWS provider
provider "aws" {
  region = "us-west-2"  # Replace with your desired region
}

# Create a security group for the instance
resource "aws_security_group" "instance_sg" {
  name        = "instance-sg"
  description = "Security group for the Red Hat instance"

  # Add inbound rules to allow SSH and HTTP access
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Create a Red Hat instance
resource "aws_instance" "redhat_instance" {
  ami           = "ami-0123456789abcdef0"  # Replace with the Red Hat AMI ID
  instance_type = "t2.micro"
  key_name      = "my-key-pair"
  security_group_ids = [aws_security_group.instance_sg.id]

  tags = {
    Name = "Red Hat Instance"
  }
}

# Create a database
resource "aws_db_instance" "database" {
  engine               = "postgres"
  instance_class       = "db.t2.micro"
  allocated_storage    = 10
  storage_type         = "gp2"
  identifier           = "my-database"
  username             = "admin"
  password             = "password123"
  publicly_accessible = false

  tags = {
    Name = "Database"
  }
}
