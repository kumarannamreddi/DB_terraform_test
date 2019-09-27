provider "AWS" {
	region = "us-east-1"
	version = "2.9.1"
}

resource "aws-instance" "DB_Test_MN"
	ami = "${var.ami_id}"
	instance_type = "t2.large"
	key_name = "ctestmt"
	subnet_id = "${data.aws_subnet.subnet.id}"
	security_groups = "${data.aws_secuirty-group.security.id}"
	

tags = {
	Name = "CTE-DB-Test_MN"
	applicationid = "CTE"
	department = "CTE"
	env = "nonprod"
	}
}

resource "aws_security_group" "cust_nfs_db" {
  name        = "cust_nfs_db"
  description = "Allow NFS outbound traffic"
  vpc_id      = "${var.vpc_id}"

  egress {
    from_port       = 4046
    to_port         = 4046
    protocol        = "tcp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
  egress {
    from_port       = 635
    to_port         = 635
    protocol        = "tcp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
  egress {
    from_port       = 4049
    to_port         = 4049
    protocol        = "tcp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
  egress {
    from_port       = 4045
    to_port         = 4045
    protocol        = "tcp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
  egress {
    from_port       = 2049
    to_port         = 2049
    protocol        = "udp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
  egress {
    from_port       = 111
    to_port         = 111
    protocol        = "tcp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
  egress {
    from_port       = 111
    to_port         = 111
    protocol        = "udp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
  egress {
    from_port       = 2049
    to_port         = 2049
    protocol        = "tcp"
    self            = "true"
    cidr_blocks     = ["0.0.0.0/0"]
}
}

resource "aws_security_group" "cust_dns_db" {
  name        = "cust_dns_db"
  description = "Allow DNS  traffic"
  vpc_id      =  "${var.vpc_id}"

  ingress {
    from_port       = 53
    to_port         = 53
    protocol        = "udp"
    cidr_blocks     = ["172.16.0.0/12", "143.199.0.0/16", "10.0.0.0/8"]
 }
  ingress {
    from_port       = 53
    to_port         = 53
    protocol        = "tcp"
    cidr_blocks     = ["172.16.0.0/12", "143.199.0.0/16", "10.0.0.0/8"]
 }
  egress {
    from_port       = 53
    to_port         = 53
    protocol        = "tcp"
    cidr_blocks     = ["172.16.0.0/12", "143.199.0.0/16", "10.0.0.0/8"]
 }
  egress {
    from_port       = 53
    to_port         = 53
    protocol        = "udp"
    cidr_blocks     = ["172.16.0.0/12", "143.199.0.0/16", "10.0.0.0/8"]
 }
}

resource "aws_security_group" "cust_ssh_db" {
  name              =  "cust_ssh_db"
  description       = "Allow ssh traffic"
  vpc_id            = "${var.vpc_id}"

  ingress {
    from_port       = 22
    to_port         = 22
    protocol        = "tcp"
    cidr_blocks     = ["172.16.0.0/12", "143.199.0.0/16", "10.0.0.0/8", "10.146.0.0/16"]
 }
  egress {
    from_port       = 22
    to_port         = 22
    protocol        = "tcp"
    cidr_blocks     = ["172.16.0.0/12", "143.199.0.0/16", "10.0.0.0/8", "10.146.0.0/16"]

}
}

resource "aws_security_group" "cust_1521" {
  name              = "cust_1521"
  description       = "Allow default Oracle port traffic"
  vpc_id            = "${var.vpc_id}"

 ingress {
   from_port        = 1521
   to_port          = 1521
   protocol         = "tcp"
   cidr_blocks      = ["143.199.99.100/31", "10.146.52.0/22"]
 }
 ingress {
   from_port        = 1521
   to_port          = 1521
   protocol         = "tcp"
   cidr_blocks      = ["143.199.99.100/31", "10.146.52.0/22"]
 }
 ingress {
   from_port        = 4904
   to_port          = 4904
   protocol         = "tcp"
   cidr_blocks      = ["143.199.99.100/31"]
 }
 ingress {
   from_port        = 3872
   to_port          = 3872
   protocol         = "tcp"
   cidr_blocks      = ["143.199.99.100/31"]
 }
 egress {
   from_port        = 1521
   to_port          = 1521
   protocol         = "tcp"
   cidr_blocks      = ["10.146.55.0/25", "143.199.99.100/31"]
 }
 egress {
   from_port        = 4904
   to_port          = 4904
   protocol         = "tcp"
   cidr_blocks      = ["143.199.99.100/31"]
 }
 egress {
  from_port         = 3872
  to_port           = 3872
  protocol          = "tcp"
  cidr_blocks       = ["143.199.99.100/31"]
 }
}

data "aws_subnet" "subnet" {
	id = "subnet.id"
}
data "aws_secuirty_group" "security" {
	id = "${var.security_group_id}"
}
