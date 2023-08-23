# # Copyright 2016 The Kubernetes Authors.
# #
# # Licensed under the Apache License, Version 2.0 (the "License");
# # you may not use this file except in compliance with the License.
# # You may obtain a copy of the License at
# #
# #     http://www.apache.org/licenses/LICENSE-2.0
# #
# # Unless required by applicable law or agreed to in writing, software
# # distributed under the License is distributed on an "AS IS" BASIS,
# # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# # See the License for the specific language governing permissions and
# # limitations under the License.

# FROM golang:1.10.0
# RUN go get github.com/codegangsta/negroni \
#            github.com/gorilla/mux \
#            github.com/xyproto/simpleredis
# WORKDIR /app
# ADD ./main.go .
# RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# FROM scratch
# WORKDIR /app
# COPY --from=0 /app/main .
# COPY ./public/index.html public/index.html
# COPY ./public/script.js public/script.js
# COPY ./public/style.css public/style.css
# CMD ["/app/main"]
# EXPOSE 3000
# Use an official Go runtime as a base image
FROM golang:1.16.0

# Set the working directory inside the container
WORKDIR /app

# Copy go.mod and go.sum files to the container
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy the rest of the source code to the container
COPY . .

# Build the binary
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Use a minimal base image for the final image
FROM scratch

# Set the working directory for the final image
WORKDIR /app

# Copy the binary from the build stage
COPY --from=0 /app/main .

# Copy other necessary files
COPY ./public/index.html public/index.html
COPY ./public/script.js public/script.js
COPY ./public/style.css public/style.css

# Define the command to run when the container starts
CMD ["/app/main"]

# Expose the necessary port
EXPOSE 3000

