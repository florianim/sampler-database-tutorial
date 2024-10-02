# Introduction

## Overview

In DevOps monitoring and observability are key to ensure a system's reliability and performance. One of the most-used systems that require monitoring are databases, where timely access to all the data must be guaranteed. As setting up monitoring tools is often complicated and tedious, in this tutorial you will learn how to use Sampler.

Sampler is a lightweight, terminal-based monitoring tool for various usecases that does not need a complex setup. Instead it uses a simple YAML file for the configuration. Therefore, sampler is a quick and effective way for monitoring in various usecases. Simply put, Sampler executes a specified shell command with a given rate and then continuously visualizes the command's output on the command line.

Here you will learn, how to use sampler for monitoring various database systems, including MySQL, PostgreSQL and MongoDB. After installing these database systems as well as sampler itself you will learn how to create a YAML configuration file for sampler to visualize the databases' performance measures. 